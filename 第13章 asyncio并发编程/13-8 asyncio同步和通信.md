```
# asyncio_lock
# total =0
 
# async def add():
#     #1.dosomething1
#     #2.io操作
#     #1.dosomething3
#     global total
#     for i in range(1000000):
#         total +=1

# async def desc():
#     global total
#     for i in range(1000000):
#         total-=1

# if __name__=="__main__":
#     import asyncio
#     tasks = [add(),desc()]
#     loop = asyncio.get_event_loop()
#     loop.run_until_complete(asyncio.wait(tasks))
#     print(total)
'''分隔符'''
#以下是演示代码，跑不通
import asyncio
from asyncio import Lock,Queue
# async def  async with  async for   Lock Queue
cache = {}
lock =Lock()

async def get_stuff(url):
    async with lock:
        if url in cache:
            return cache[url]
        stuff = await aiohttp.request("GET",url)
        cache[url]=stuff
        return stuff


async def parse_stuff():
    stuff = await get_stuff()
    # do some parsing

async def use_stuff():
    stuff = await get_stuff()
    # use stuff to do something interesting

tasks = [parse_stuff(),use_stuff()]
loop =asyncio.get_event_loop()
loop.run_until_complete(asyncio.wait(tasks))

