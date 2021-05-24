```
# def gen_func():
#     #1.可以产出值，2.可以接受值（调用方传递进来的值）
#     html = yield "http://projectsedu.com"
#     print(html)
#     yield 2
#     yield 3
#     return "bobby"

# #throw close 方法


# if __name__=="__main__":
#     gen = gen_func()
#     #1.启动生成器有两种方法；next，send
#     #在调用send发送非none值之前，我们必须启动一次生成器，方式有两种：1.gen.send(none) 2.next(gen)
#     url = next(gen)
#     html ="bobby111"
#     print(gen.send(html))
#     #send方法可以传递进入生成器内部，同时还可以重新启动生成器执行到下一个
#     print(next(gen))
#     # print(next(gen))
#     # print(next(gen))

##throw close 方法 ----------
# def gen_func():
#     #1.可以产出值，2.可以接受值（调用方传递进来的值）
#     try:
#         yield "http://projectsedu.com"
#     except Exception:
#         pass
#     yield 2
#     yield 3
#     return "bobby"

# if __name__=="__main__":
#     gen = gen_func()
#     print(next(gen))
#     gen.close()
#     print(next(gen))
#     # generatorexit是继承自baseexception 而不是exception

#gen throw 

def gen_func():
    #1.可以产出值，2.可以接受值（调用方传递进来的值）
    try:
        yield "http://projectsedu.com"
    except Exception as e:
        pass
    yield 2
    yield 3
    return "bobby"

if __name__=="__main__":
    gen = gen_func()
    print(next(gen))
    gen.throw(Exception,"download error")
    print(next(gen))
    # gen.throw(Exception,"download error")
    # generatorexit是继承自baseexception 而不是exception
