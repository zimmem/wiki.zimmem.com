---
layout: post
title: JDK动态代理
description: 
tags: jdk proxy
---

# [{{ page.title }}][1]

接口

	public interface Interface {
	
	    void doSomething();
	}


实现类

	public class ImplClass implements Interface{
	
	    @Override
	    public void doSomething() {
	        System.out.println("====do something ===");
	        
	    }
	}

代理handle

	public class ProxyHandler implements InvocationHandler {
	
	    public Object target;
	
	    public ProxyHandler(Object target) {
	        this.target = target;
	    }
	
	    @Override
	    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
	        System.out.println("==run before ivoke method== ");
	        Object result = method.invoke(target, args);
	        System.out.println("==run after ivoke method==");
	        return result;
	    }
	
	}

sample

	 public static void main(String[] args) {
	        Interface target = new ImplClass();
	        Interface newProxyInstance = (Interface) Proxy.newProxyInstance(target.getClass().getClassLoader(),
	                                                                        new Class[] { Interface.class },
	                                                                        new ProxyHandler(target));
	        newProxyInstance.doSomething();
	    }

输出

	==run before ivoke method== 
	====do something ===
	==run after ivoke method==


[1]:    {{ page.url}}  ({{ page.title }})