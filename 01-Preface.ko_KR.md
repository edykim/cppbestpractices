# 서문

C++ 모범 사례: 포크 가능한 코딩 표준 문서

이 문서는 C++의 모범 사례를 공동으로 논의하는데 그 의미가 있습니다. 이 문서는 *이펙티브 C++ (Effective C++)* (Meyers)와 *C++ 코딩의 정석 (C++ Coding Standards)* (Alexandrescu, Sutter)을 보완합니다. 책에서 논하지 않은, 낮은 수준의 세부적인 사항도 다루며 명확한 스타일을 추천하려고 합니다. 또한 전반적인 코드 품질을 준수하는 방법도 함께 다룹니다.

모든 경우에 간결함과 명료함을 선호합니다. 한 선택지 대신 다른 선택지를 선호하는 경우라면 왜 그 선택지를 선호하는지 예를 만들어 설명하는 방식을 선호합니다. 필요에 따라서만 설명이 붙습니다.

<a rel="license" href="http://creativecommons.org/licenses/by-nc/4.0/"><img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by-nc/4.0/88x31.png" /></a><br /><span xmlns:dct="http://purl.org/dc/terms/" href="http://purl.org/dc/dcmitype/Text" property="dct:title" rel="dct:type">C++ Best Practices</span> by <a xmlns:cc="http://creativecommons.org/ns#" href="http://cppbestpractices.com" property="cc:attributionName" rel="cc:attributionURL">Jason Turner</a> is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-nc/4.0/">Creative Commons Attribution-NonCommercial 4.0 International License</a>.

*면책 조항*

이 문서는 개인적인 경험에 기반합니다. 이 문서를 100% 동의할 필요는 없습니다. 이 내용은 [GitHub](https://github.com/lefticus/cppbestpractices)에 올라와 있으므로 포크해서 본인 용도로 사용하거나 모두와 공유하고 싶은 변경점이 있다면 변경 제안을 제출해도 됩니다.

이 책은 오라일리 비디오 [Learning C++ Best Practices](http://shop.oreilly.com/product/0636920049814.do)에서 영감을 받았습니다.
