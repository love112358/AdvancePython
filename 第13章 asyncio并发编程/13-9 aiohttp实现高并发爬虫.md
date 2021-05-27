```
#aiohttp github中搜索aiohttp sanic 高并发服务器 github  aiomysql
#pip install aiohttp
#pip install pyquery
#pip install aiomysql
#asyncio 爬虫，去重，入库
import asyncio
import re
import aiohttp
import aiomysql
from pyquery import pyquery
stopping = False
start_url = "http://www.jobbole.com/"
waitting_urls = []
seen_urls = set()#内存太大了

sem = asyncio.Semaphore(1)

async def fetch(url,session):
    async with sem:   
        try:
            async with session.get(url) as resp:
                print("url status: {}".format(resp.status))
                if resp.status in [200,201]:
                    data = await resp.text()
                    return data
        except Exception as e:
            print(e)

def extract_urls(html):
    urls = []
    pq = PyQuery(html)
    for link in pq.items("a"):
        url = link.attr("href")
        if url and url.startswith("http") and url not in seen_urls:
            urls.append(url)
            waitting_urls.append(url)
        return urls

async def init_urls(url,session):
    html = await fetch(url,session)
    seen_urls.add(url)
    extract_urls(html)

async def artitle_handler(url,session,pool):
    #获取文章详情，并解析入库
    html = await fetch(url,session)
    seen_urls.add(url)
    extract_urls(html)
    pq = PyQuery(html)
    title = pq("title").text()
    async with pool.acquire() as conn:
        async with conn.cursor() as cur:
            await cur.execute("SELECT 42;")
            #需要新建数据库aiomysql_test,-》新建表(名:title,类型：varchar,长度：200)-》表名为article_test，
            insert_sql = "insert into article_test(title) values('{}')".format(title)
            await cur.execute(insert_sql)

async def consumer(pool):
    async with aiohttp.ClientSession() as session:
        while not stopping:
            if len(waitting_urls)==0:
                await asyncio.sleep(0.5)
                continue
            url = waitting_urls.pop()
            print("start get url:{}".format(url))
            if re.match('http"//.*?jobbole.com/\d+/',url):
                if url not in seen_urls:
                    asyncio.ensure_future(artitle_handler(url,session,pool))
            else:
                if url not in seen_urls:
                    asyncio.ensure_future(init_urls(url,session))
                    await asyncio.sleep(0.5)

async def main(loop):
    #等待mysql连接建立
    pool=await aiomysql.create_pool(host='127.0.0.1',port=3306,user='root',password='root',db='aiomysql_test',loop=loop,charset='uft8',autocommit=True)
    async with aiohttp.ClientSession() as session:
        html = await fetch(start_url,session)
        seen_urls.add(start_url)
        extract_urls(html)
    
    asyncio.ensure_future(consumer(pool))

if __name__=="__main__":
    loop = asyncio.get_event_loop()
    asyncio.ensure_future(main(loop))
    loop.run_forever()                
