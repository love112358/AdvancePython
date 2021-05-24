```
#python3 新增的yield from语法
# from itertools import chain
# my_list = [1,2,3]
# my_dict = {"bobby1":"http://projectsedu.com",
#             "bobby2":"http://www.imooc.com",}


# def my_chain(*args,**kwargs):
#     for my_iterable in args:
#         for value in my_iterable:
#             yield value

# for value in chain(my_list,my_dict,range(5,10)):
#     print(value)
# for value in my_chain(my_list,my_dict,range(5,10)):
#     print(value)

#yield from iterable
# def g1(iterable):
#     yield iterable

# def g2(iterable):
#     yield from iterable

# for value in g1(range(10)):
#     print(value)

# for value in g2(range(10)):
#     print(value)

# def my_chain(*args,**kwargs):
#     for my_iterable in args:
#         yield from my_iterable
#         # for value in my_iterable:
#         #     yield value
# for value in my_chain(my_list,my_dict,range(5,10)):
#     print(value)


# def g1(gen):
#     yield from gen

# def main():
#     g = g1()
#     g.send(None)
#main 调用方 g1（委托生成器）gan子生成器
#1. yield from 会在调用与子生成器之间建立一个双向通道


##一个案例分析
# final_result = {}

# def sales_sum(pro_name):
#     total =0
#     nums=[]
#     while True:
#         x = yield
#         print(pro_name+"销量：",x)
#         if not x:
#             break
#         total +=x
#         nums.append(x)

#     return total,nums

# def middle(key):
#     while True:
#         final_result[key] = yield from sales_sum(key)
#         print(key+"销量统计完成！！.")

# def main():
#     data_sets = {
#         "bobby牌面膜":[1200,1500,3000],
#         "bobby牌手机":[28,55,98,108],
#         "bobby牌大衣":[280,560,778,70],
#     }
#     for key,data_set in data_sets.items():
#         print("start key:",key)
#         m=middle(key)
#         m.send(None)
#         for value in data_set:
#             m.send(None) #给协程传递每一组的值
#         m.send(None)
    
#     print("final_result:",final_result)
# if __name__=="__main__":
#     main()

'''fenggefu '''

# def sales_sum(pro_name):
#     total =0
#     nums=[]
#     while True:
#         x = yield
#         print(pro_name+"销量：",x)
#         if not x:
#             break
#         total +=x
#         nums.append(x)

#     return total,nums

# if __name__=="__main__":
#     my_gen = sales_sum("bobby牌手机")
#     my_gen.send(None)
#     my_gen.send(1200)
#     my_gen.send(1500)
#     my_gen.send(3000)
#     try:
#         my_gen.send(None)
#     except StopIteration as e:
#         result = e.value
#         print(result)

##yield from how.py
# 代码太多了
