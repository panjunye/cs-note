# Retrofit源码分析

``` java
public <T> T create(final Class<T> service) {
    Utils.validateServiceInterface(service);
    if (validateEagerly) {
        eagerlyValidateMethods(service);
    }
    return (T) Proxy.newProxyInstance(service.getClassLoader(), new Class<?>[] { service },
        new InvocationHandler() {
            private final Platform platform = Platform.get();

            @Override public Object invoke(Object proxy, Method method, @Nullable Object[] args)
                throws Throwable {
            // If the method is a method from Object then defer to normal invocation.
            if (method.getDeclaringClass() == Object.class) {
                return method.invoke(this, args);
            }
            if (platform.isDefaultMethod(method)) {
                return platform.invokeDefaultMethod(method, service, proxy, args);
            }
            ServiceMethod<Object, Object> serviceMethod =
                (ServiceMethod<Object, Object>) loadServiceMethod(method);
            OkHttpCall<Object> okHttpCall = new OkHttpCall<>(serviceMethod, args);
            return serviceMethod.callAdapter.adapt(okHttpCall);
            }
        });
}
```

Retrofit的create方法，传入我们定义的接口，形如FooApi.class，然后对接口的方法进行代理，对于接口中的每一个方法，获取它的参数、返回类型，注解，然后根据这些参数调用OkHttp完成.

对于每一个接口中的方法，以方法为key，生成对应的ServiceMethod，


``` java
ServiceMethod<?, ?> loadServiceMethod(Method method) {
    ServiceMethod<?, ?> result = serviceMethodCache.get(method);
    if (result != null) return result;

    synchronized (serviceMethodCache) {
        result = serviceMethodCache.get(method);
        if (result == null) {
        result = new ServiceMethod.Builder<>(this, method).build();
        serviceMethodCache.put(method, result);
        }
    }
    return result;
}
```


# Dagger2
Dagger2是一个依赖注入的框架。
1.Dagger2通过apt在编译时生成代码，避免使用反射影响效率。
2.对于每个可以注入的对象，Dagger2生成该对象的工厂类；同时一个对象也依赖其他对象，因此它的工厂类也持有其他对象的工厂类，如此一层一层嵌套，构成整个依赖结构。