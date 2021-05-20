```
#Lock Rlock semaphores condition
from threading import Lock,RLock # 可重入的锁
total = 0
lock =Lock()
def add():
    global total
    global lock
    for i in range(1000000):
        lock.acquire()
        total += 1
        lock.release()
def desc():
    global total
    global lock
    for i in range(1000000):
        lock.acquire()
        total -= 1
        lock.release()

import threading
thread1 = threading.Thread(target=add)
thread2 = threading.Thread(target=desc)
thread1.start()
thread2.start()
def add1(a):
    a+=1
def desc1(a):
    a-=1
"""
add
1. load a a =0
2. load 1 1
3. +  1
4. 赋值给a  a=1
"""

"""
desc
"""
1. load a   a =0
2. load 1  1
3. 1  -1
4. 赋值给a  a=-1
所以 a=1 或者-1
"""
import dis
print(dis.dis(add1))
print(dis.dis(desc1))


#thread1.join()
#thread2.join()
#print(total)
