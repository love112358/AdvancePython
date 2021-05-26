```
import asyncio
def callback(sleep_times,loop):
    print("sleep {} success {}".format(sleep_times,loop.time()))

def stoploop(loop):
    loop.stop()

if __name__=="__main__":
    loop = asyncio.get_event_loop()
    now = loop.time()
    #call_soon方法
    # loop.call_soon(callback,2)
    # loop.call_soon(callback,2)
    #call_later方法
    # loop.call_later(2,callback,2)
    # loop.call_later(1,callback,1)
    # loop.call_later(3,callback,3)

    #call_at 方法
    loop.call_at(now+2,callback,2,loop)
    loop.call_at(now+1,callback,1,loop)
    loop.call_at(now+3,callback,3,loop)
    loop.call_soon(callback,4,loop)
    # loop.call_soon(stoploop,loop)
    loop.run_forever()
