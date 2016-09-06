---
title: SOAP调用webservice
date: 2016-09-06 12:59:17
tags: java
---

## 如果提供一个远程的web服务，本地一般的几种调用方式
* HTTP的方式
* Axis2(生成客户端方式 ,RPC,DOM)
* SOAP 


 一般的webservice请求这三种方式都可以实现,但是如果服务端采用了某些限制，例如禁用了http访问,以及采用cookie保持会话的这种奇葩方式，建议你使用[ksoap2-android-assembly-2.5.7-jar-with-dependencies.jar](http://www.java2s.com/Code/JarDownload/ksoap2/ksoap2-android-assembly-2.5.7-jar-with-dependencies.jar.zip "Title") 这个包来实现与服务端的通信



参考：http://blog.csdn.net/lovekwf/article/details/44205101


```java
 private Object coreCallResponse(String methodName, List<Object[]> params) {

    	String result = null;// 方法的返回值
		String SOAP_ACTION = NS_URL + methodName;
		SoapObject requestObject = new SoapObject(NS_URL, methodName);
		if (params != null) {
            //数组类型的参数采用PropertyInfo来序列化，否则会出现无法序列化的异常
			if (methodName.equals("wsQueryYktByUserCodeArray")) {
				for (Object[] oArr : params) {
					for (Object o : oArr) {
						PropertyInfo p = new PropertyInfo();
						p.setName("string");
						p.setValue(o);
						requestObject.addProperty(p);
					}
				}

			} else {
				for (Object[] kv : params) {
					requestObject.addProperty(kv[0].toString(), kv[1]);
				}
			}
		}
		SoapSerializationEnvelope envelope = new SoapSerializationEnvelope(
				SoapEnvelope.VER11);
		envelope.dotNet = false; 
		envelope.bodyOut = requestObject;
		HttpTransportSE hts = new HttpTransportSE(SOAP_URL);
		Object o = null;
		try {
			LinkedList<HeaderProperty> reqHeaders = new LinkedList<HeaderProperty>();
			// 若已经带有cookie,则每次访问WebService时,要带有这个cookie。
			if (mSessionHeader != null) {
				reqHeaders.add(new HeaderProperty("Cookie", mSessionHeader
						.getValue()));

			}
            // 访问WebService
			@SuppressWarnings("rawtypes")
			List headerList = hts.call(SOAP_ACTION, envelope, reqHeaders);
			if (headerList != null) {
				for (Object headerElement : headerList) {
					HeaderProperty headerProperty = (HeaderProperty) headerElement;
					String headerKey = headerProperty.getKey();
					String headerValue = headerProperty.getValue();
					if (headerKey != null
							&& headerKey.equalsIgnoreCase("set-cookie")) {
						// 服务端发送过来的set-cookie项
						mSessionHeader = headerProperty;
					}
				}

			}

			o = envelope.getResponse();
			result = o.toString();
		} catch (SoapFault e) {
			e.printStackTrace();
		} catch (IOException e) {
			e.printStackTrace();
		} catch (XmlPullParserException e) {
			e.printStackTrace();
		}

		return result;
	}

```