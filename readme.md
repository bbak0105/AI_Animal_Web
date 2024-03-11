# Teachable Machine을 활용한 닮은 동물상 찾기 웹 사이트 제작

⚠️ nelify에 배포되어 직접 확인하실 수 있습니다. 가끔 로딩이 길어지면 [Predict] 버튼을 한번 더 눌러주세요. <br/>
https://serene-speculoos-fcd4b9.netlify.app/

<br/>

## 📌 Preivew

<img width="1030" alt="스크린샷 2024-03-10 오후 10 27 30" src="https://github.com/bbak0105/AI_Animal_Web/assets/66405572/d62f4193-a1f9-420d-8377-fa19aa100736">
<img width="1256" alt="스크린샷 2024-03-11 오후 12 20 54" src="https://github.com/bbak0105/AI_Animal_Web/assets/66405572/3ae09270-5fef-4566-a4a7-878d07d3f8c3">

<br/>
<br/>

## 📌 Skills
### Language
<a><img src="https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-badge&logo=JavaScript&logoColor=white"/></a>

### IDE
<a><img src="https://img.shields.io/badge/Goorm-66FFFF.svg?&style=for-the-badge&logoColor=white"/></a>

### Skills
<a><img src="https://img.shields.io/badge/HTML5-E34F26?style=for-the-badge&logo=html5&logoColor=white"/></a>
<a><img src="https://img.shields.io/badge/CSS-239120?&style=for-the-badge&logo=css3&logoColor=white"/></a>
<a><img src="https://img.shields.io/badge/jQuery-0769AD?style=for-the-badge&logo=jquery&logoColor=white"/></a>

### Deploy
<a><img src="https://img.shields.io/badge/Netlify-00C7B7?style=for-the-badge&logo=netlify&logoColor=white" /></a>

<br/>

## 📌 Descriptions
⚠️ 다소 급하게 진행헀던 팀프로젝트여서, 코드 리팩토링이 전혀 되어있지 않습니다.  <br/>
예시로, const로 보통 작성하는 편인데 var로 작성되어 있습니다. 전역변수도 다소 많이 사용한 편입니다.<br/>
헤당 영상을 참고하여 만들었습니다. https://youtu.be/OI3fZJHQF8Y?feature=shared

<br/>

### `Photo Upload`
> ✏️ 사진 업로드를 할 수 있는 공간입니다. 업로드가 되면 readURL 함수를 호출하여 FileReader로 파일을 읽습니다.

#### ・ HTML

```HTML
<div class="file-upload">
    <button
        class="file-upload-btn"
        type="button"
        onclick="$('.file-upload-input').trigger( 'click' )"
    >
        Add Image
    </button>

    <div class="image-upload-wrap">
        <input
            class="file-upload-input"
            type="file"
            onchange="readURL(this);"
            accept="image/*"
        />
        <div class="drag-text">
            <h3>Drag and drop a file or select add Image</h3>
        </div>
    </div>
    ...
</div>
```

#### ・ JavaScript

```javaScript
function readURL(input) {
  if (input.files && input.files[0]) {
    const reader = new FileReader();

    reader.onload = function (e) {
        $('.image-upload-wrap').hide();
        $('.file-upload-image').attr('src', e.target.result);
        $('.file-upload-content').show();
        $('.image-title').html(input.files[0].name);
    };

    reader.readAsDataURL(input.files[0]);
    initList();
  } else {
    removeUpload();
  }
}
```

---

### `Model Init`
> ✏️ Teachable Machine 모델을 초기화 하는 곳입니다. 예측을 진행하기 전, 티쳐블 머신 모델을 웹에 적용하는 코드입니다. <br/> 
> model: Teachable Machine에서 학습한 모델을 저장할 변수입니다. <br/>
> labelContainer: 결과 레이블을 표시할 HTML 요소를 나타내는 변수입니다. <br/>
> maxPredictions: 모델이 예측할 수 있는 클래스의 최대 개수를 나타내는 변수입니다.

#### ・ JavaScript

```javaScript
let model, webcam, labelContainer, maxPredictions;
const URL = 'https://teachablemachine.withgoogle.com/models/1k-_K6jgp/';
const modelURL = URL + 'model.json';
const metadataURL = URL + 'metadata.json';

// 초기화
async function modelInit() {
  document.getElementById("progress").style.display = 'block';

  model = await tmImage.load(modelURL, metadataURL);
  maxPredictions = model.getTotalClasses();

  // append elements to the DOM
  labelContainer = document.getElementById('label-container');

  for (let i = 0; i < maxPredictions; i++) {
    // and class labels
    labelContainer.appendChild(document.createElement('div'));
  }
}

```

---

### `Predict`
> ✏️ 예측을 진행하는 코드입니다. 앞서 모델을 초기화 한 후에, 예측을 진행하고 각 동물에 맞는 probability를 보여줍니다.

#### ・ JavaScript

```javaScript
async function predict() {
  modelInit();

  // predict can take in an image, video or canvas html element
  const prediction = await model.predict(image, false);
  
  let max = {
    per: 0,
    index: 0
  };
  
  for (let i = 0; i < maxPredictions; i++) {
    let initClass = i + 1;
    let initClassIndex = "#list"+initClass;

    $(initClassIndex).removeClass("active");
    
    if(prediction[i].probability.toFixed(2) > max.per) {
      max.per = prediction[i].probability.toFixed(2);
      max.index = i;
    }
    
    list1.innerHTML = "🐶 Dog 🐶  " + prediction[0].probability.toFixed(2);
    list2.innerHTML = "🐈 Cat 🐈  " + prediction[1].probability.toFixed(2);
    list3.innerHTML = "🐻 Bear 🐻  " + prediction[2].probability.toFixed(2);
    list4.innerHTML = "🦖 Dinosaur 🦖  " + prediction[3].probability.toFixed(2);
    list5.innerHTML = "🐰 Rabbit 🐰  " + prediction[4].probability.toFixed(2);
  }
  
  let maxIndex = max.index + 1;
  let listIndex = "#list"+maxIndex;
  
  $(listIndex).addClass("active");
  document.getElementById("progress").style.display = 'none';
}
```
