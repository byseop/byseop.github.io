---
layout: post
title: Swiper slider 여러개 사용하기
feature-img: "assets/img/snippet/swiper.png"
tags: [snippet, Swiper, slider]
---
개인적으로 가장 모던하고 편한 슬라이더는 Swiper 라고 생각한다.  
Swiper 슬라이더를 한페이지에 여러개 사용해야 할때 유용한 스니펫.
  


~~~javascript
$(".swiper-container").each(function(index, element){
    var $this = $(this);
    $this.addClass('instance-' + index);

    var swiper = new Swiper('.instance-' + index, {
        observer: true,
        observeParents: true,
        slidesPerView : 5,
        navigation: {
            nextEl: $('.instance-' + index).siblings('.swiper-button-next'),
            prevEl: $('.instance-' + index).siblings('.swiper-button-prev'),
        },
        scrollbar: {
            el: $('.instance-' + index).siblings('.swiper-scrollbar'),
            hide: false,
        },
        watchOverflow: true
    });
});
~~~