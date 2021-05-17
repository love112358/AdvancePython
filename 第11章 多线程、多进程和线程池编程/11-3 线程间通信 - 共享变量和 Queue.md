## 数据共享
- 共享变量

使用一个全局变量, 然后不同线程可以访问并修改这个变量

- queue
```
from queue import Queue

detail_url_queue = Queue(maxsize=1000)

queue.put("http://projectsedu.com/{id}".format(id=i))
url = queue.get()

```
##线程间的通信     ##以下by me   
```
variables.py
detail_url_list=[]
thread_queue.py
import time
import threading
from xx import variables
detail_url_list = []
def get_datail_html():
    detail_url_list = variables.detail_url_list
    #爬取文章详情页
    while True:
        if len(variables.detail_url_list):
            url = detail_url_list.pop()
            # for url in detail_url_list:
            print("get detail html started")
            time.sleep(2)
            print("get detail html end")
  
def get_detail_url(detail_url_list):
    #爬取文章列表页
    while True:
        print("get detail url started")
        time.sleep(4)
        for i in range(20):
            detail_url_list.append("http://projectsedu.com/{id}".format(id=i))

        print("get detail url end")

if __name__ == "__main__":
    thread_detail_url = threading.Thread(target=get_detail_url,args=(detail_url_list))
    for i in range(10):
        html_thread = threading.Thread(target=get_ditail_url,args=(detail_url_list))
        html_thread.start()
    start_time = time.time()
    print("last time:{}".format(time.time()-start_time))
    
thread_queue_test.py
from queue import   Queue
import time
import threading
def get_datail_html(queue):
    #爬取文章详情页
    while True:
        url =queue.get()
            # for url in detail_url_list:
            print("get detail html started")
            time.sleep(2)
            print("get detail html end")
  
def get_detail_url(queue):
    #爬取文章列表页
    while True:
        print("get detail url started")
        time.sleep(4)
        for i in range(20):
            queue.put("http://projectsedu.com/{id}".format(id=i))

        print("get detail url end")

if __name__ == "__main__":
detail_url_queue = Queue(maxsize=1000)
    thread_detail_url = threading.Thread(target=get_detail_url,args=(detail_url_queue))
    for i in range(10):
        html_thread = threading.Thread(target=get_ditail_url,args=(detail_url_queue))
        html_thread.start()
    start_time = time.time()
    print("last time:{}".format(time.time()-start_time))

    
    
















