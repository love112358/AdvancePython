```
import os
import time 
#fork 只用于linux和Unix中
#进程的数据是隔离的
# print("bobby")
# pid = os.fork()
# # print("bobby")
# if pid ==0:
#     print("子进程 {}，父进程是：{}".format(os.getpid(),os.getppid()))

# else:
#     print("我是父进程：{}".format(pid))

# time.sleep(2)

from concurrent.futures import ProcessPoolExecutor
import multiprocessing

#多进程变成
import time
def get_html(n):
    time.sleep(n)
    return n

if __name__=="__main__":
    # progress = multiprocessing.Process(target=get_html,args=(2,))
    # print(progress.pid)
    # progress.start()
    # 等待所有任务完成
    # print(progress.pid)
    # progress.join()
    # print("main progress end")

    #使用进程池
    pool = multiprocessing.Pool(multiprocessing.cpu_count())
    # result = pool.apply_async(get_html,args=(3,))
    # pool.close()
    # pool.join()
    # print(result.get())
    #imap
    for result in pool.imap(get_html,[1,5,3]):
        print("{} sleep success".format(result))
    # imap_unordered 谁先完成就打印谁
    # for result in pool.imap_unordered(get_html,[1,5,3]):
    #     print("{} sleep success".format(result))
