---
layout: post
title:  "keychainitemwrapper"
date: 2017-05-31 22:26
categories: iOS
tags:  Keychain 
author: Babamba
---

* content
{:toc}

keychainitemwrapper라이브러리와 Keychain에 대한 요약입니다.

keychainitemwrapper
===

1.keychainitemwrapper 라이브러리를 사용한 이유


* 키체인 관련 API가 C함수로 정의되어 사용이 까다로워 보다 쉽게 사용하기 위해 apple에서 지원하는 클래스 파일인 keychainitemwrapper를 사용하였다.

* keychain item 생성 및 초기화

```objc

KeychainItemWrapperkeyChainWrapperInLoginManager = [[KeychainItemWrapper alloc] initWithIdentifier:_RECORD_KEYCHAINITEMWRAPPER_KEY accessGroup:nil];

/** 
 * 내부 앱에서만 사용할 경우 accessGroup:nill로 하면되고, 
 * 공유할 경우 accessGroup:@"???????.com.yourcompany.hoho" 이런식으로 해야한다.
 */

```

* keychain item에 추가 수정

```objc

 [self.keyChainWrapperInLoginManager setObject:token forKey:(__bridge id)kSecValueData ];
 /**
  * kSecAttrAccount		 : Account 정보
  * kSecAttrLabel		 : 라벨 정보
  * kSecAttrDescription : 설명
  * kSecValueData		 : Data
  */   
   
```

* keychain item에서 불러오기

```objc

[self.keyChainWrapperInLoginManager objectForKey:(__bridge id)kSecValueData];

```

* keychain item 초기화 시키기

```objc

[self.keyChainWrapperInLoginManager resetKeychainItem];

```

Keychain
====

1.Keychain의 정의

* Mac OS와 iOS에서 사용하는 '암호화된 저장 장소'
* 사용자가 애플리케이션을 사용할 때 서버나 암호화된 웹등을 사용하게 되는데 이때 사용자 인증과정이 필요하다.
* 애플리케이션을 사용할 때마다 이 인증과정을 거치게 되는데 이를 간소화 하기 위해 Keychain이 도입되었다. 

2.Keychain의 특징

* 기본적으로 애플리케이션은 자신의 키체인에만 접근할 수 있다.
* 키체인이 잠기면 아무도 접근할 수 없다.
* 키체인은 provisioning profile로 사용경로를 구분한다. 따라서 같은 앱이라도 개발단계에서 profile을 바꾸면 키체인 조회가 불가능하므로 같은 앱에서는 동일한 provisioning profile을 사용해야 한다.
* 앱을 삭제해도 단말기를 초기화하지 않는 한 키체인에 저장된 Data는 지워지지 않는다.
* Use에게는 Keychain item이 보이지 않는다.
* 보안이 필요한 Data(password, private key등)는 암호화되어 저장되고, 보안이 필요하지 않는 Data(Cerificate 등)는 암호화하지 않고 저장한다.

3.키체인의 구조

* Keychain item : 키체인에 저장된 Data 단위. 키체인은 하나 이상의 Keychain item을 가질 수 있다.
* item class : 저장할 Data의 종류. ID/PW, Cerificate, Generic password 등.
* Attributes : keychain item을 설명하는 특성. item class에 따라 설정할 수 있는 attributes가 달라진다.

4.키체인의 공유

* 앱 개발시 서로 다른앱끼리 keychain item을 공유해야되는 경우가 있는데(예 : 구글 드라이브에서 구글 스프레드시트로 이동시), 이때 Keychain Groups를 사용하여 서로 다른앱에서 keychain item을 공유할 수 있도록 만들 수 있다.

5.키체인을 사용하는 경우?

* 보안이 중요한 data를 저장할 필요가 있을 때 사용하는 것이 좋다. 
* NSUserDefult에 보안이 중요한 데이터를 저장하지 안흔 이유는 암호화 되지 않은 상태에서 앱에 저장되기 때문이다.

  * [참고1 "Are You Storing Sensitive Data in NSUserDefaults? Stop Doing That!"](https://www.andyibanez.com/nsuserdefaults-not-for-sensitive-data/)
  * [참고2 "Keychain 그리고 KeychainItemWrapper](http://yolojeb.tistory.com/22)
  * [참고3 "Keychain Services Programming Guide"](https://developer.apple.com/library/content/documentation/Security/Conceptual/keychainServConcepts/01introduction/introduction.html#//apple_ref/doc/uid/TP30000897)


ARC - bridge
====

위의 keychainitemwrapper의 예제 코드를 보면 __bridge라는 녀석이 나오는데, 이는 keychain이 C함수로 만들어져 있는것과 관련이 있다. 

ARC환경에서 objective - c 타입과 C 타입의 비슷한 타입(NSArray Vs. CFArrayRef)이 존재하고 이들간 형 변환이 가능하다. 

형변환을 한 경우, ARC가 원래의 타입을 몰라 retain 등을 할 때 문제가 생길 여지가 있다. 따라서 이를 해결하지 위해 __brige를 사용하다.

자세한 내용은 아래와 같다.

1.NSType VS CFType

* NS는 NextStep의 약자이며, CF는 CoreFoundation의 약자이다.

 * NSType : objective - c 타입 (예 : NSData, NSArray ...) 
 * CFType : C 타입 (예 : CFArrayRef ... )

2.비 ARC 환경에서의 retain과 release 방법

* NSType의 경우 

```objc

NSDate *date = [[NSDate alloc] init]; //retain 생성

NSDate *otherObject = [date retain];

[otherObject release];

[date release];

```

* CFType의 경우
 
 * CFRetain() 함수와 CFRelease() 함수가 retain과 release 기능을 제공한다.

```objc

CFArrayRef array = CFArrayCreate(...);  // CFRetain 발생

CFArrayRef somePtr = CFRetain(array);

CFRelease(somePtr);

CFRelease(array);

```
3.자동 브릿지 타입

* NSType과 CFType사이에는 NSArray와 CFArrayRef, NSDictionary와 CFDictionaryRef 등과 같이 비슷한 타입이 존재한다.

* 이 연관된 두 타입은 자동으로 타입 캐스팅을 이용해 서로간 타입을 이동한다. 이를 Automatically Bridged Type이라 한다.

* ARC 환경에서 포인터의 원래 타입으로 모른다. 이때 형변환을 했다면 retain을 거는 등의 처리를 해야하는데 어디에 할지 ARC는 혼한해 한다.


4.일반적인 형변환 시의 브릿지 __bridge

```objc

(__bridge T) op

```

* __bridge : op를 T타입으로 브릿지 캐스팅을 할 때 사용하는 키워드. 즉, T가 NSType이면, op는 CFType이며 반대의 경우도 성립한다.
* ARC는 이 경우 op에 별도의 retain이나 release를 걸지 않는다.

```objc

NSArray *array = (__bridge NSArray *)cfArray;

```

 * 위의 코드는 일반적인 objective - c 코드로 ARC하에서 코딩하는 것 처럼 동작한다.

 [참고1 "ARC - 브릿지 이야기"](http://seorenn.blogspot.kr/2015/01/objective-c-arc.html) 