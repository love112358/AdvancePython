```
# from concurrent.futures import ThreadPoolExecutor
# 未来对象，task的返回容器
# #线程池，为什么用线程池
# #主线程中获取某一个线程的状态或者某一个任务的状态，以及返回值
# #当一个线程完成的时候我们的主线程能立即知道
# #futures可以让多线程和多线程编码借口一致

# import time

# def get_html(times):
#     time.sleep(times)
#     print("get page {} success".format(times))
#     return times

# executor = ThreadPoolExecutor(max_workers=2)
# #通过submit函数提交执行的函数到线程池中，submit是立即返回
# task1 = executor.submit(get_html,(3))
# task2 = executor.submit(get_html,(2))

# #done方法用于判定某个任务是否已经完成
# print(task1.done())
# print(task2.cancel())
# time.sleep(3)
# print(task1.done())
# #result 方法可以获取task的执行结果
# print(task1.result())


# '''分界线'''
# from concurrent.futures import ThreadPoolExecutor,as_completed
# #线程池，为什么用线程池
# #主线程中获取某一个线程的状态或者某一个任务的状态，以及返回值
# #当一个线程完成的时候我们的主线程能立即知道
# #futures可以让多线程和多线程编码借口一致

# import time

# def get_html(times):
#     time.sleep(times)
#     print("get page {} success".format(times))
#     return times

# executor = ThreadPoolExecutor(max_workers=2)
# #通过submit函数提交执行的函数到线程池中，submit是立即返回
# # task1 = executor.submit(get_html,(3))
# # task2 = executor.submit(get_html,(2))
# #要获取已经成功的task的返回
# urls = [3,2,4]
# # all_task=[executor.submit(get_html,(url)) for url in urls]
# # for future in as_completed(all_task):
# #     data = future.result()
# #     print("get {} page success".format(data))
# # 通过executor的map获取已经完成task的值
# for data in executor.map(get_html,urls):
#     print("get {} page".format(data))

'''分界线'''
from concurrent.futures import ThreadPoolExecutor,as_completed,wait,FIRST_COMPLETED
#线程池，为什么用线程池
#主线程中获取某一个线程的状态或者某一个任务的状态，以及返回值
#当一个线程完成的时候我们的主线程能立即知道
#futures可以让多线程和多线程编码借口一致

import time

def get_html(times):
    time.sleep(times)
    print("get page {} success".format(times))
    return times

executor = ThreadPoolExecutor(max_workers=2)
#通过submit函数提交执行的函数到线程池中，submit是立即返回
# task1 = executor.submit(get_html,(3))
# task2 = executor.submit(get_html,(2))
#要获取已经成功的task的返回
urls = [3,2,4]
all_task=[executor.submit(get_html,(url)) for url in urls]
wait(all_task,return_when=FIRST_COMPLETED)
print("main")
# for future in as_completed(all_task):
#     data = future.result()
#     print("get {} page success".format(data))
# 通过executor的map获取已经完成task的值
# for data in executor.map(get_html,urls):
#     print("get {} page".format(data))



