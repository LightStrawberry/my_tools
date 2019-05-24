

自定义排序示例:

def cmp(a,b):
    str1 = str(a)+str(b)
    str2 = str(b)+str(a)
    if str1 > str2:
        return 1
    elif str1 < str2:
        return -1
    else:
        return 0

if __name__ == '__main__':
    nums = [1,3,20,15]
    nums.sort(key=functools.cmp_to_key(cmp))
    print(nums)
    
原理:
上面一个cmp_to_key函数就把cmp函数变成了一个参数的key函数，那么这个函数背后究竟做了什么，看下源码就知道了.源代码如下

def cmp_to_key(mycmp):
    """Convert a cmp= function into a key= function"""
    class K(object):
        __slots__ = ['obj']
        def __init__(self, obj):
            self.obj = obj
        def __lt__(self, other):
            return mycmp(self.obj, other.obj) < 0
        def __gt__(self, other):
            return mycmp(self.obj, other.obj) > 0
        def __eq__(self, other):
            return mycmp(self.obj, other.obj) == 0
        def __le__(self, other):
            return mycmp(self.obj, other.obj) <= 0
        def __ge__(self, other):
            return mycmp(self.obj, other.obj) >= 0
        __hash__ = None
    return K
