```
#通过非阻塞io实现http请求
#requests -> urlib -> socket
#使用了非阻塞io完成http请求
# import socket
# from urllib.parse import urlparse


# #使用了非阻塞io完成http请求
# def get_url(url):
#     # 解析url
#     url = urlparse(url)
#     host = url.netloc
#     path = url.path
#     if path == "":
#         path = "/"

#     #建立socket连接
#     client = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
#     client.setblocking(False)
#     try:
#         client.connect((host, 80))
#     except BlockingIOError as e:
#         pass

#     # 发送请求的格式很重要
#     while True:
#         try:
#             client.send("GET {} HTTP/1.1\r\nHost:{}\r\nConnection:close\r\n\r\n".format(path, host).encode("utf8"))
#             break
#         except OSError as e:
#             pass
#     data = b""
#     while True:
#         try :
#             d = client.recv(1024)
#         except BlockingIOError as e:
#             continue
#         if d:
#             data += d
#         else:
#             break

#     data = data.decode("utf8")
#     # 去掉request header
#     html_data = data.split("\r\n\r\n")[1]
#     print(html_data)
#     client.close()

# if __name__ == "__main__":
#     import time
#     start_time = time.time()
#     for url in range(20):
#         url = "http://shop.projectsedu.com/goods/{}/".format(url)
#         get_url(url)
#     print(time.time()-start_time)
'''分隔符'''
#使用select完成http请求
#select+回调+事件循环
#并发性高
#使用单线程
import socket 
from urllib.parse import urlparse
from  selectors import DefaultSelector,EVENT_READ,EVENT_WRITE
selector = DefaultSelector()
urls = []
stop =False
class Fetcher:
    def connected(self,key):
        selector.unregister(key.fd)
        self.client.send("GET {} HTTP/1.1\r\nHost:{}\r\nConnection:close\r\n\r\n".format(self.path, self.host).encode("utf8"))
        selector.register(self.client.fileno(),EVENT_READ,self.readable)

    def readable(self,key):
        d = self.client.recv(1024)
        if d:
            self.data += d
        else:
            selector.unregister(key.fd)
            data = self.data.decode("utf-8")
            # 去掉request header
            html_data = data.split("\r\n\r\n")[1]
            print(html_data)
            self.client.close()
            urls.remove(self.spider_url)
            if not urls:
                global stop
                stop =True


    def get_url(self,url):
        # 解析url
        self.spider_url = url
        url = urlparse(url)
        self.host = url.netloc
        self.path = url.path
        self.data = b""
        if self.path == "":
            self.path = "/"

        #建立socket连接
        self.client = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
        self.client.setblocking(False)

        try:
            self.client.connect((self.host, 80))
        except BlockingIOError as e:
            pass
        

        #注册
        selector.register(self.client.fileno(),EVENT_WRITE,self.connected)

def loop():
    #事件循环，不停的请求socket的状态并调用对应的回调函数
    #1.select 本身是不支持register模式的
    #2.socket状态变化以后的回调是由程序员完成的
    while not stop:
        ready = selector.select()
        for key,mask in ready:
            call_back = key.data
            call_back(key)
    #回调+事件循环+select（poll\epoll）

if __name__=='__main__':
    fetcher =Fetcher()
    import time
    start_time = time.time()
    for url in range(20):
        url = 'http://shop.projectsedu.com/goods/{}/'.format(url)
        urls.append(url)
        fetcher = Fetcher()
        fetcher.get_url(url)
    loop()
    print(time.time()-start_time)


