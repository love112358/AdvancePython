```
"""第一版本"""
import threading
class XiaoAi(threading.Thread):
    def __init__(self,lock):
        super().__init__(name="小爱")
        self.lock = lock
    def run(self):
        self.lock.acquire()
        print("{}: 在".format(self.name))
        self.lock.release()
        self.lock.acquire()
        print("{}: 好啊".format(self.name))
        self.lock.release()
        
class TianMao(threading.Thread):
    def __init__(self,lock):
        super().__init__(name="天猫精灵")
        self.lock = lock
    def run(self):
        self.lock.acquire()
        print("{}:小爱同学 ".format(self.name))
        self.lock.release()
        self.lock.acquire()
        print("{}:我们来对古诗吧 ".format(self.name))
        self.lock.release()
if __name__ =="__main___":
    lock = threading.Lock()
    xiaoai =XiaoAi()
    tianmao =TianMao()
    tianmao.start()
    xiaoai.start()
    
"""通过condition方法"""
from threading import condition
class XiaoAi(threading.Thread):
    def __init__(self,cond):
        super().__init__(name="小爱")
        self.cond = cond
    def run(self):
        with self.cond:
            self.cond.wait()
            print("{}: 在".format(self.name))
            self.cond.notify()
            self.cond.wait()
            
            print("{}:好啊 ".format(self.name))
            self.cond.notify()
            self.cond.wait()
            
            print("{}:君住长江尾 ".format(self.name))
            self.cond.notify()
            self.cond.wait()
            
            print("{}:共饮长江水 ".format(self.name))
            self.cond.notify()
            self.cond.wait()
            
            print("{}:此恨何时已 ".format(self.name))
            self.cond.notify()
            self.cond.wait()
            
            print("{}:定不负相思意 ".format(self.name))
            self.cond.notify()
            self.cond.wait()
    
class TianMao(threading.Thread):
    def __init__(self,cond):
        super().__init__(name="天猫精灵")
        self.cond = cond
    def run(self):
        with self.cond:
            print("{}:小爱同学 ".format(self.name))
            self.cond.notify()
            self.cond.wait()
            print("{}:我们来对古诗吧 ".format(self.name))
            self.cond.notify()
            self.cond.wait()
            
            print("{}:我住长江头 ".format(self.name))
            self.cond.notify()
            self.cond.wait()
            
            print("{}:日日思君不见君 ".format(self.name))
            self.cond.notify()
            self.cond.wait()
            
            print("{}:此水几时休 ".format(self.name))
            self.cond.notify()
            self.cond.wait()
            
            print("{}:只愿君心似我心 ".format(self.name))
            self.cond.notify()
            self.cond.wait()
            

if __name__ =="__main___":
    cond = threading.Condition()
    xiaoai =XiaoAi(cond)
    tianmao =TianMao(cond)
    xiaoai.start()
    tianmao.start()


