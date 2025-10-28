
# [JS]

#### "체크 상태가 반영되지 않음"

**문제 상황**
- `moneyPrint()`에서 `.prop('checked', true)`로 라디오/체크 상태를 바꿔도  
  `element.innerHTML`(또는 `$(...).html()`)은 DOM *property*가 아니라 HTML *attribute*를 직렬화합니다.
- 따라서 `checked`가 DOM 프로퍼티로만 설정되어 있으면 `html()`로 복사할 때 체크 상태가 반영되지 않습니다.

---

### ✅ 프린트에 반영할 값 저장 함수

```js
function moneyPrint() {
    // (기존 값 복사 / 체크 설정 부분)
    var locationVal = $("input[name='location']:checked").val();
    var gubunVal = $("input[name='gubun']:checked").val();
    var typesVal  = $("input[name='types']:checked").val();
    var discountVal = $("input[name='discount']:checked").val();

    if (locationVal) $('input[name="location"][value="' + locationVal + '"]').prop('checked', true);
    if (gubunVal)    $('input[name="gubun"][value="' + gubunVal + '"]').prop('checked', true);
    if (typesVal)    $('input[name="types"][value="' + typesVal + '"]').prop('checked', true);
    if (discountVal) $('input[name="discount"][value="' + discountVal + '"]').prop('checked', true);
    
    // (...추가)
}
```

---

### ✅ 프린트 함수

```js
    var screenPrint = function(data){
        
        moneyPrint(); // 프린트 금액 전달

        //    대상 영역 내부의 input/textarea 상태를 'attribute'로 동기화
        //    이렇게 하면 .html()로 가져올 때 체크/값이 반영됩니다.
        var $area = $("#" + data);

        // 라디오/체크박스: checked 프로퍼티를 checked attribute로 복사
        $area.find('input[type="radio"], input[type="checkbox"]').each(function(){
            var $this = $(this);
            if ($this.prop('checked')) {
                $this.attr('checked', 'checked');
            } else {
                $this.removeAttr('checked');
            }
        });

        // 생략...
    }
```
