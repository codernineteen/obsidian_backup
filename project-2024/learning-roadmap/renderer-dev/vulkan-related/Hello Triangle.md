
## Resource management

 In cpp, we can mangage memory that is dynamically allocated with [RAII](https://en.cppreference.com/w/cpp/language/raii) pattern.
 when we create Vulkan objects, we use 'vkCreateXXX' or 'vkAllocateXXX' provided by the api. It is trivial that we should destroy the resources allocated by the functions using counter-part destroyer like 'vkDestroyXXX'.
 To achieve this, we can expand standard smart pointers or build our new custom allocators.


## Vk instance
This instance makes a connection between our application and vulkan library
To give useful information to driver, we can fill out application information properties in 'VKApplicationInfo' struct.
There is a 'pNext' field in VKxxx struct which can point to extension information.

A lot of information in Vulkan is passed through another structs.
For example,
```cpp
void createInstance() {
	VkApplicationInfo appInfo{};
	appInfo.sType = VK_STRUCTURE_TYPE_APPLICATION_INFO; // set app info enum variant to struct Type field
	appInfo.pApplicationName = "VKRenderer"; // application name
	appInfo.applicationVersion = VK_MAKE_VERSION(1,0,0); // version specifier
	appInfo.pEngineName = "NO ENGINE";
	appInfo.engineVersion = VK_MAKE_VERSION(1, 0, 0);
	appInfo.apiVersion = VK_API_VERSION_1_3; // VKRenderer using sdk version 1.3.268

	
	VkInstanceCreateInfo createInfo{};
	createInfo.sType = VK_STRUCTURE_TYPE_INSTANCE_CREATE_INFO;
	createInfo.pApplicationInfo = &appInfo;
}

```


Because Vulkan is a platoform-agnostic(independent) API, we need to give additional extension about window system.

## VKResult
Without a need to store results of processing vulkan routine, we can just validate the result of routine with a type 'VKResult'. (quite similar to rust programming language's Result haha)
When it was successful, the type will be marked as VK_SUCCESS.
```cpp
if (vkCreateInstance(&createInfo, /*custom allocator*/ nullptr, &instance) != VK_SUCCESS) {
    throw std::runtime_error("failed to create an instance");
}
```


## Validation layer

Using vulkan means that there are lots of things that developer should handle.
To avoid errors hard to debug from mistakes, we can use elegant system called as 'validation layers'
