## **这是一个简易的Json文件解析器，支持深浅拷贝**



使用方法：

```cpp
#include "src/sonelement.h"
#include <iostream>
using json = ttw::json;

int main(){
	json::JsonElement obj;
	obj["null"] = nullptr;
	obj["int"] = 123;
	obj["float"] = 123.123; 
	obj["string"] = "string";
	obj["boo"] = true;
	{
		json::JsonElement arr(json::JsonElement::JsonType::ARRY);
		arr.append(123);
		arr.append(nullptr);
		arr.append("string");
		arr.append("false");
		arr.append(123.123);
		//obj["arr"] = arr;  因为默认为浅拷贝，如何在arr作用域进行使用，会触发段错误，需要使用std::move(arr)，把arr的释放权交给obj
		obj["arr"] = std::move(arr);		
	}
	
	std::cout << obj << std::endl;

	return 0;
}

```



有着与JSONCPP相似的函数功能

```cpp
obj["null"].isNull();
obj["string"].isString();
obj["arr"].isArry();
...
'''
obj["string"].asString();
obj["int"].asInt();

obj["arr"].toString();

```



解析json数据

```cpp
#include <string>
#include <iostream>
#include "src/jsonelement.h"
#include "src/reader.h"

using json = ttw::json;

int main(){
	std::string data = R"(
  	{
    	"name": ttw,
    	"age": 10000,
  	}
	)"
	json::JsonElement val;
	json::Reader reader(data);
	if(reader.paerser(val)){
		std::cout << val << std::endl;
	}

}
```

