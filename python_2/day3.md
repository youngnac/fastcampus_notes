2017-2-2

#CLASS

##Object Oriented Programming
>class보다 instancec가 우선


객체:
변수
###Class
####property - getter/setter
>private 속성을 읽고 쓰기위해 property를 사용
>getter, setter
>

####Inheritance
`class Restaurant(Shop):`
> Restaurant class가 Shop class 상속
>
>비슷한 기능을 수행하나, 약간의 추가적인 기능이 필요한 다른 클래스가 필요할 경우 기존의 클래스를 상속받은 새 클래스를 사용하는 형태로 문제를 해결할 수 있다.

#####Override
```python
class restaurant(shop):
    def __init__(self, name, shop_type, address, avg_price):
        super().__init__(name, shop_type, address)
        self.avg_price = avg_price
    def get_info(self):
        ori = super().get_info()
        print('get_info, ori: [{}]'.format(ori))
        return ori.replace('상점','SSSSS')+'\n avg price: {}'.format(self.avg_price)
```

```python
@property
def name(self):
    return self.__name

@name.setter
def name(self, new_name):
    self.__name = new_name
    print('Set new name ({})'.format(self.__name))
```


```python
class shop:
    description = 'python shop class'
    def __init__(self, name,shop_type, address):
        self.__name = name
        self.__shop_type = shop_type
        self.__address = address

#    def change_type(self, new_type):
#       self.__shop_type = new_type
    def show_info(self):
        print('상점정보: ({})\n\t유형: {}\n\t주소:{}'.format(self.__name, self.__shop_type,self.__address))

    @classmethod
    def change_description(cls, new_description):
        cls.__description = new_description

    def get_name(self):
        return self.__name

    def set_name(self, new_name):
        self.__name = new_name
#    @staticmethod
#    def print_test():
#        print('stat')

    @property
    def name(self):
        #return self.__name[:1] + '**'
        return self.__name
    @name.setter
    def name(self, new_name):
        self.__name = new_name
        print('set new name: {}'.format(self.__name))


shop1 = shop('롯데리아!', 'fastfood','seoul')
shop2 = shop('mcdonlad','fastfood','busan')
shop3 = shop('indibrad','clothing','jeju')

#print(shop1.get_name())
#shop1.set_name('newname')
#print(shop1.get_name())
print(shop1.name)
```

