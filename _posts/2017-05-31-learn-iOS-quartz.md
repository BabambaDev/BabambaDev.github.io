---
layout: post
title:  "Part1 Ch1 스위프트"
date: 2017-05-31 10:00
categories: iOS
tags:  Core Graphics 
author: Babamba
---

* content
{:toc}

Quartz2D Overview 부분 해석입니다.
구글번역기의 도움을 받았습니다.
의역 오역 넘쳐나니 주의하세요.


##문서 번역
 
Core Graphics(Quartz 2D)
===

##Ch.1 Quartz 2D 개요


* 2차원적으로 그리기 위한 엔진
* 사용 예
 * 경로 기반 그림(path-based drawing)
 * 투명도 (painting with transparency)
 * 명암 (shading)
 * 그림자 그리기 (drawing shadows)
 * 투명층 (transparency layers)
 * 색상관리 (color management)
 * anti-aliased rendering
 * PDF 문서 생성 (PDF document generation)
 * PDF 메타 데이터 액세스 (PDF metadata access)

* 다른 모든 graphics와 imaging technologies(Core Image, Core Video, OPenGL, Quick Time) 함께 작동할 수 있다.

##1. The Page

* Quartz 2D 이미징을 위해 painter’s model를 사용한다. 
* painter’s model에서, 각 연속적인 드로잉 작업은 출력 "캔버스"에 "Paint" 레이어를 적용한다.(이를 Page라 부른다.)
* Page의 Paint는 추가적인 드로잉 작업을 통해 더 많은 Paint를 겹쳐 수정될 수 있다. 
* Page에 그려진 객체는 더 많은 paint를 겹치는것을 제외하고는 수정될 수 없다.
* 이 모델로 적은 기초요소(primitives)로 부터 극도의 정교한 이미지를 만들 수 있다.

	![그림 1-1](https://developer.apple.com/library/content/documentation/GraphicsImaging/Conceptual/drawingwithquartz2d/Art/painters_model.gif) <그림 1-1>

* 그림 1-1은 painter’s model의 작동 방식을 보여준다.
* 그림 상단 부분의 이미지를 얻으려면, 좌측의 형태를 먼저 그려지고 꽉찬 형태가 그려지면 얻어진다.
* 꽉찬 형태에 첫번째 형태가 겹쳐지면, 첫번째 형태의 둘레를 제외하고는 모두 가려진다.
* 하단 부분의 이미지는 반대 순서로 그리고, 꽉찬 형태가 먼저 그려 진다.
* painter’s model은 그리는 순서가 중요하다.
* Page는 실제 용지이거나 가상용지(pdf), 비트맵 이미지일 수 있다.
* Page의 특성은 사용하는 graphics context에 따른다.

##2. Drawing Destinations: The Graphics Context

* Graphics Context는 불투명한 데이터 타입([CGContextRef](https://developer.apple.com/reference/coregraphics/cgcontextref?language=objc))이다.
* 데이터 타입에는 Quartz 정보가 압축 되어있다. 그 정보는 출력장치(PDF파일, 비트맵, 디스플레이의 윈도우)에 이미지를 그리는 데에 사용한다.
* Gtaphics context 내부의 정보에는 graphics drawing 파라미터와 page의 paint에 기기 장치에 표현이 포함된다.
* Quartz의 모든 객체들은 graphics context에 그려지거나 포함된다.

![그림 1-2](https://developer.apple.com/library/content/documentation/GraphicsImaging/Conceptual/drawingwithquartz2d/Art/draw_destinations.gif)<그림 1-2>

* 그림 1-2와 같이 graphics context를 드로인 대상으로 생각할 수 있다. 
* Quratz로 그릴때, 모든 디바이스의 특성들은 사용하는 특정 Graphics context 유형에 포함한다.
* 즉, 다른 디바이스에 단순히 Quartz 드로잉 루틴의 같은 시퀀스에 다른 graphics context 제공하는것에 의해 같은 이미지를 그릴 수 있다. 

* 이 graphics context들을 애플리케이션에 활용할 수 있다.
 * 비트맵 graphics context에 RGB 컬러, CMYK 컬러, grayscale을 그릴 수 있다. 비트맵은 사각형 픽셀의 어레이(또는 래스터)이다. 각 픽셀은 이미지의 점을 표현한다. 비트맵 이미지들은 samepled image로 불린다.
 * layer context([CGLayerRef](https://developer.apple.com/reference/coregraphics/cglayerref))는 대상이 다른 graphics context에 관련된 offscreen drawing이다. 레이어를 만든 graphics context에 레이어를 그릴때 최적화 되어있도록 설계 되었다. layer context는 비트맵 graphics context 보다 offscreen drawing에서 더 좋은 선택일 수 있다.

##3. Quartz 2D Opaque Data Types

* Opaque Data Type : 해당 데이터 타입의 내부 정보가 외부 인터페이스로 모두 노출되지 않은 데이터 타입

* Quartz 2D API는 graphics context 뿐만 아니라 다양한 Opaque Data Type들을 정의한다. 이 API가 Core Graphics 프레임워크의 일부이기 때문에, 데이터 타입과 그들에 작동되는 루틴들은 CG 접두어를 사용한다.
* Quartz 2D는 애플리케이션이 특정 드로잉 아웃풋을 만들기 위해 작동하는 Opaque Data Type로부터 객체를 생성한다. 그림 1-3 은 Quartz 2D가 제공하는 세 개의 객체에 드로잉 연산을 적용 할 때 얻을 수있는 결과의 종류를 보여준다.
 * PDF 페이지 객체를 생성하고, 회전 연산을 그래픽 콘텍스트에 적용하고, Quartz 2D가 그래픽 콘텍스트에 페이지를 드로잉하도록 요청하여 PDF 페이지를 회전하고 디스플레이 할 수있다.
 * 패턴 오브젝트를 생성하고, 패턴을 구성하는 모양을 정의하고, graphics context에 그리는 패턴을 페인트로 사용하도록 Quartz 2D를 설정함으로써 패턴을 그릴 수 있다.
 * shading 객체를 생성하는것, shading 각 포인트에서 색상을 결정하는 함수를 제공하는것,그리고 Quartz 2D에 shading을 색을 채워 사용하도록 요청는것에 의해 축 방향 또는 방사형 음영으로 영역을 채울 수 있다.

		![그림 1-3](https://developer.apple.com/library/content/documentation/GraphicsImaging/Conceptual/drawingwithquartz2d/Art/drawing_primitives.gif)<그림 1-3 Opaque Data Type는 드로잉 Quratz 2D 기본요소의 기준이다.>
		
* Quratz 2D의 Opaque Data Type는 다음을 따른다.

	|CGPathRef|채우거나 선을 그어 path를 생성하기 위해 벡터 그래픽에 사용 |
|:--|:--|
|CGImageRef|사용자가 제공한 샘플데이트를 기반으로 비트맵 이미지와 비트맵 이미지 마스크를 나타내는데 사용 |
|CGLayerRef|반복 드로잉과 offscreen 드로잉에 사용할 수 있는 드로잉 레이어를 나타내기 위해 사용|
|CGPatternRef| 반복 드로잉에 사용 |
|CGShadingRef, CGGradientRef| gradients를 paint하기 위해 사용 |
|CGFunctionRef| 부동 소수점 arguments의 임의의 수를 취하는 콜백 함수를 정의하기 위해 사용|
|CGColorRef, CGColorSpaceRef|색 해석 방법을 Quratz에 알리기 위해 사용 |
|CGImageSourceRef, CGImageDestinationRef|Quratz 안 밖으로 이동하는 데이터에 사용한다|
|CGFontRef|text를 그리는데 사용됨|
|CGPDFDictionaryRef, CGPDFObjectRef, CGPDFPageRef, CGPDFStream, CGPDFStringRef, CGPDFArrayRef|PDF 메타 데이터에 대한 액세스를 제공|
|CGPDFScannerRef, CGPDFContentStreamRef|PDF메타데이터를 분석한다.|
|CGPSConverterRef|PsotScript를 PDF로 변환하는 데 사용된다. iOS에서는 사용불가|

##4. Graphics States

* Quartz는 current graphics state의 파라미터에 따라 드로잉 작업결과를 수정한다.
* graphics state는 드로잉 루틴에 대한 인수(argument)로 사용되는 파라미터를 포함한다. 
* graphics context를 그리는 루틴은 결과를 결정하는 방법을 결정하기 위해 graphics state를 찾아본다.
* 예를 들어, 색을 채우는 설정을 위한 함수를 부를때 현재 그래픽 상태에 저장된 값을 수정한다.
* 현재 그래픽 상태의 다른 일반적으로 사용되는 요소에는 선 너비, 현재 위치 및 텍스트 글꼴 크기가 포함됩니다.
* Graphics context는 graphics state의 스택을 포함한다.
* Quratz가 graphics context를 생성할 때 스택은 비어있다.
* graphics state를 저장할 때, quartz는 스택으로 현재 graphics state의 사본을 푸쉬한다.
* graphics state를 복원할 때, quart는 스택 맨위의 graphics state를 pop한다.
* pop된 state는 현재 graphics state가 된다.

* 현재 graphics state를 저장하기위해, 스택에 현재 grahics state가 사본이 푸쉬되는 CGContextSaveState 함수가 사용된다.
* 이전의 저장된 graphics state를 회복하기 위해,현재 graphics state를 스택 맨위의 graphics state로 대체하는 CGContextRestoreGState 함수를 사용한다. 
* 현재 드로잉 환경의 모든 측면이 Graphics state의 요소들은 아니라는것을 유의하라.
* 예로, 현재 path는 graphics state의 부분으로 고려되지 않는다. 그리고 CGContextSaveGState 함수를 호출 할 떄 저장되지 않는다.

표 1-1 Graphics state 관련된 parameter

| Parameters| 이 장에서 논의 되는 것|
|:--|:--|
|Current transformation matrix (CTM)|[Transforms](https://developer.apple.com/library/content/documentation/GraphicsImaging/Conceptual/drawingwithquartz2d/dq_affine/dq_affine.html#//apple_ref/doc/uid/TP30001066-CH204-TPXREF101)|
|Clipping area|[Paths](https://developer.apple.com/library/content/documentation/GraphicsImaging/Conceptual/drawingwithquartz2d/dq_paths/dq_paths.html#//apple_ref/doc/uid/TP30001066-CH211-TPXREF101)|
|Line: width, join, cap, dash, miter limit|[Paths](https://developer.apple.com/library/content/documentation/GraphicsImaging/Conceptual/drawingwithquartz2d/dq_paths/dq_paths.html#//apple_ref/doc/uid/TP30001066-CH211-TPXREF101)|
|Accuracy of curve estimation (flatness)|[Paths](https://developer.apple.com/library/content/documentation/GraphicsImaging/Conceptual/drawingwithquartz2d/dq_paths/dq_paths.html#//apple_ref/doc/uid/TP30001066-CH211-TPXREF101)|
|Anti-aliasing setting|[Graphics Contexts](https://developer.apple.com/library/content/documentation/GraphicsImaging/Conceptual/drawingwithquartz2d/dq_context/dq_context.html#//apple_ref/doc/uid/TP30001066-CH203-TPXREF101)|
|Color: fill and stroke settings|[Color and Color Spaces](https://developer.apple.com/library/content/documentation/GraphicsImaging/Conceptual/drawingwithquartz2d/dq_color/dq_color.html#//apple_ref/doc/uid/TP30001066-CH205-TPXREF101)|
|Alpha value (transparency)|[Color and Color Spaces](https://developer.apple.com/library/content/documentation/GraphicsImaging/Conceptual/drawingwithquartz2d/dq_color/dq_color.html#//apple_ref/doc/uid/TP30001066-CH205-TPXREF101)|
|Rendering intent|[Color and Color Spaces](https://developer.apple.com/library/content/documentation/GraphicsImaging/Conceptual/drawingwithquartz2d/dq_color/dq_color.html#//apple_ref/doc/uid/TP30001066-CH205-TPXREF101)|
|Color space: fill and stroke settings|[Color and Color Spaces](https://developer.apple.com/library/content/documentation/GraphicsImaging/Conceptual/drawingwithquartz2d/dq_color/dq_color.html#//apple_ref/doc/uid/TP30001066-CH205-TPXREF101)|
|Text: font, font size, character spacing, text drawing mode|[Text](https://developer.apple.com/library/content/documentation/GraphicsImaging/Conceptual/drawingwithquartz2d/dq_text/dq_text.html#//apple_ref/doc/uid/TP30001066-CH213-TPXREF101)|
|Blend mode|[Paths](https://developer.apple.com/library/content/documentation/GraphicsImaging/Conceptual/drawingwithquartz2d/dq_paths/dq_paths.html#//apple_ref/doc/uid/TP30001066-CH211-TPXREF101) and [Bitmap Images and Image Masks](https://developer.apple.com/library/content/documentation/GraphicsImaging/Conceptual/drawingwithquartz2d/dq_images/dq_images.html#//apple_ref/doc/uid/TP30001066-CH212-TPXREF101)|

##5. Quartz 2D Coordinate Systems

* 그림 1-4에서 보여주는 데 좌표계는 Page의 위치와 크기를 나타내는데 사용되는 위치 범위를 정의한다. 
* Graphics의 사용자 공간 좌표계(간단히 사용자 공간)에서의 위치와 사이즈가 특정된다. 
* 좌표는 부동소수점 값으로 정의된다.

	<그림 1-4>
	
	![그림 1-4](https://developer.apple.com/library/content/documentation/GraphicsImaging/Conceptual/drawingwithquartz2d/Art/quartz_coordinates.gif)

* 서로 다른 장치는 서로 다른 이미지 능력이 가지고 있기 때문에, graphics의 위치와 사이즈는 장치 독립적인 방식으로 정의 되어야만 한다.
* 예로, 스크린 디스플레이 장치는 인치 당 96필셀을 표시할 수는 있지만, 프린터에서는 인치당 300픽셀을 표시할 수 있다.
* 장치 레벨(이 예제에서는 96 또는 300 픽셀)에서 좌표계가 정의된다면, 공간에서 그려진 객체들은 눈에 보이는 왜곡없이 다른 장치에 재현될 수 없다. 크거나 작게 나타난다.
* Quartz는 현재 변환 행렬 또는 CTM을 사용하여 출력 장치의 좌표계(장치 공간)에 매핑하는 별도의 좌표계(사용자 공간)로 장치 독립성을 달성한다.
* matrix는 방정식에 관련된 설정을 묘사하는 효율적인 수학적인 구조이다.
* current transformation matrix는 affin transform으로 불리는 matrix의 특정 타입이다. 
* 회전 및 크기 조정 작업 (좌표계를 이동, 회전 및 크기 조정하는 계산)을 적용하여 한 좌표 공간에서 다른 좌표 공간으로 점을 매핑합니다.
* current transformation matrix는 두 번째 목적을 갖는다. 객체를 그리는 방법을 변형할 수 있다.
* 예로, 45도로 회전한 박스를 그리기 위해, 박스를 그리기 전에 page의 좌표계를 회전한다. Quartz는 회전된 좌표계를 출력장치에 그린다.
* 사용자 공간의 한점은 좌표 쌍(x,y)에 의해 표현된다. 여기서 x 는 가로 축 (왼쪽 및 오른쪽)의 위치를 ​​나타내고 y 는 세로 축 (위아래)을 나타낸다.
* 사용자 좌표 공간의 원점은 점 (0,0)이다. 원점은 그림 1-4 와 같이 page의 왼쪽 아래 구석에 있다. Quartz의 기본 좌표계에서, x 축은 페이지의 왼쪽에서 오른쪽으로 이동함에 따라 증가한다. Y 축은 페이지의 아래쪽에서 위쪽으로 이동함에 따라 값이 커진다.
* 일부 기술은 Quartz에서 사용하는 것과 다른 기본 좌표계를 사용하여 graphics state를 설정한다. Quartz는 이와 관련하여, 이러한 좌표계는 수정 된 좌표계이고 일부 Quartz 그리기 작업을 수행 할 때 이를 보완해야한다. 가장 일반적인 수정된 좌표계는 원점을 컨텍스트의 왼쪽 위 모서리에 배치하고 y 축을 페이지 아래쪽을 향하도록 변경합니다. 사용 된 특정 좌표계를 볼 수있는 몇 가지 위치는 다음과 같다.
 * Mac OS X에서는 isFlipped메소드를 재정의 하고 YES를 반환하는 NSView의 subclass. 
 * iOS에서는 UIView.
 * iOS에서는 UIGraphicsBeginImageContextWithOptions함수 를 호출하여 생성된 드로잉 컨텍스트 이다.

* UIKit에서 수정된 좌표계를 사용하는 Quartz 드로잉 context를 반환하는 이유는 UIKit이 다른 기본 좌표 규칙을 사용하기 때문이다. 생성된 Quartz 컨텍스트에 변환을 적용하여 규칙을 일치시킨다(?). 애플리케이션이 동일한 드로잉 루틴을 사용하여 UIView객체와 PDF 그래픽 콘텍스트 (Quartz로 생성되고 기본 좌표계를 사용함)를 그리기를 원하는 경우, PDF graphics context를 같은 수정된 좌표계를 받는 변형을 적용하는것이 필요하다. 이렇게하려면, 원본을 PDF context의 왼쪽 위 모서리로 변환하고 -1로 y 좌표 scale를 변환하는 변환을 적용한다.

* y좌표를 무효화하여 스케일링 변환을 사용하는것으로 Quartz 드로잉 일부 규칙이 변경된다. 예로, context에 이미지를 그리기 위해 CGContextDrawImage를 호출한 경우, 이미지들은 대상이 그려진때 변형에 의해 수정된다. 유사하게, path 드로잉 루틴들은 호가 기본 좌표계에서 시계 방향 또는 반 시계 방향으로 그려지는지 여부를 지정하는 특정 파라미터들을 수락한다. 만약 좌표계가 변형된 경우, 이미지가 거울에 반사된것 처럼 그 결과는 수정된다. 그림 1-5처럼, Quartz에 같은 파라미터들을 전달하는것으로 기본 좌표계에서 시계방향 호가 결과가 되고 같은 파라미터를 Quartz에 건네 주면 (자), 디폴트의 좌표계에 시계 방향의 호가, Y 좌표의 뒤에 반 시계 방향의 호가 변환에 의해 무효가된다.

	<그림 1-5 좌표계를 수정하면 대칭 이미지가 만들어진다.>
	
	![그림 1-5](https://developer.apple.com/library/content/documentation/GraphicsImaging/Conceptual/drawingwithquartz2d/Art/flipped_coordinates.jpg)

* Quartz 호출이 변환을 적용한 컨텍스트에 맞게 조정하는 것은 애플리케이션에 달려있다. 예로, 이미지 또는 PDF를 graphics context에 올바르게 그리려면 응용 프로그램에서 graphics context의 CTM을 일시적으로 조정해야 할 수 있습니다. iOS에서 CGImage 객체를 랩핑하여 UIImage 객체를 사용하는 경우, CTM을 수정할 필요가 없다. UIImage 객체는 UIKit에 의해 적용된 수정된 좌표계를 자동으로 보완한다.

* 중요 :  위의 설명은 iOS에서 Quartz를 직접 타깃으로하는 애플리케이션을 작성하려는 경우 이해하는 데 필수적이지만 충분하지는 않습니다. iOS 3.2 이상에서, UIKit이 응용 프로그램의 드로잉 context를 만들 때 기본 UIKit 규칙과 일치하도록 context를 추가로 변경합니다. 특히, CTM의 영향을받지 않는 패턴과 그림자는 UIKit의 좌표계와 규칙이 일치하도록 개별적으로 조정됩니다. 이 경우 애플리케이션이 UIKit에서 제공하는 컨텍스트의 동작과 일치하도록 Quartz에서 만든 컨텍스트를 변경하는 데 사용할 수있는 CTM과 동일한 메커니즘이 없습니다. 어플리케이션은 그것이 드로잉하고있는 컨텍스트의 종류를 인식하고 컨텍스트의 예상과 일치하도록 동작을 조정해야한다.

##6. Memory Management: Object Ownership

* Quartz는 객체를 reference count하는 경우 Core Foundation 메모리 관리 모델을 사용한다. 생성시 Core Foundation 객체는 reference count 1부터 시작한다. 객체를 유지하는 함수를 호출하여 reference count를 증가시키고 객체를 릴리스하는 함수를 호출하여 reference count를 감소시킵니다. reference count를 0으로 감소 시키면 객체가 해제됩니다. 이 모델을 사용하면 객체가 다른 객체에 대한 참조를 안전하게 공유 할 수 있습니다.

* 명심할 몇가지 규칙이 있다.

 * 객체를 만들거나 복사하는 경우 객체를 소유하고 있으므로 객체를 릴리스해야한다. 즉, 일반적으로 이름에 "Create"또는 "Copy"라는 단어가있는 함수에서 객체를 가져 오는 경우 객체가 그것이 완료된 때 객체를 해제해야한다. 그렇지 않으면 메모리 누수가 발생합니다.
 * 이름에 "Create"또는 "Copy"라는 단어가 포함되지 않은 함수에서 개체를 얻는 경우 해당 개체에 대한 참조를 소유하지 않으므로 이를 해제하면 안된다. 이 객체는 미래의 어떤 시점에 객체 소유자에 의해 릴리즈된다.
 * 객체를 소유하고 있지 않은 상태에서 객체를 유지할 필요가 있다면, 객체를 유지해야하고 그것이 완료되면 릴리즈해야 한다. 객체와 관련된 Quartz 2D 함수를 사용하여 객체를 유지하고 해제한다. 예로, CGColorspace 개체에 대한 참조를 받은 경우, 필요한 객체를 유지하고 릴리즈하기위해 CGColorSpaceRetain 함수와 CGColorSpaceRelease함수를 사용한다. 코어 파운데이션 함수 CFRetain과 CFRelease를 사용할 수 있다. 그러나 함수에 NULL을 넘겨주지 않도록 주의해야한다.