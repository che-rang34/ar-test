# AR 캐릭터 프로토타입

바닥이나 테이블에 3D 캐릭터를 놓고, 버튼으로 손 흔들기·점프를 재생하는 웹 AR이에요.
`index.html` 파일 하나로 동작해요. 설치나 빌드는 필요 없어요.

- **Android:** Chrome에서 바로 동작해요 (WebXR)
- **iPhone:** Variant Launch 키를 넣으면 동작해요 (아래 4번)
- 지금 캐릭터는 three.js 샘플 로봇(RobotExpressive)이에요. 나중에 내 모델로 바꾸면 돼요 (아래 5번).

---

## 1. GitHub에 올리기

1. https://github.com 에서 가입해요.
2. 오른쪽 위 **+** → **New repository**를 눌러요.
   - Repository name: 예) `ar-test`
   - **Public**을 선택해요 (무료 Pages는 Public이어야 해요)
   - **Create repository**를 눌러요.
3. 만들어진 화면에서 **uploading an existing file** 링크를 누르고, `index.html`과 `README.md`를 끌어다 놓은 뒤 **Commit changes**를 눌러요.

## 2. 링크 만들기 (GitHub Pages)

1. 저장소 상단의 **Settings** → 왼쪽 **Pages**로 가요.
2. **Branch**를 `main` / `/(root)`로 고르고 **Save**를 눌러요.
3. 1~2분 기다린 뒤 새로고침하면 위쪽에 주소가 나와요.
   `https://내아이디.github.io/ar-test/`
4. 이 주소를 폰으로 열면 돼요. 카톡으로 보내거나 QR 코드를 만들어도 돼요.

파일을 고쳐서 다시 올리면 1~2분 뒤 같은 주소에 반영돼요.

## 3. Android로 테스트하기

- **Chrome**으로 여세요. 삼성 인터넷은 안 될 수 있어요.
- 처음에 "Google Play 서비스 AR" 설치 안내가 나오면 설치하세요.
- 카메라 권한은 **허용**해 주세요.
- **내 공간에서 보기** → 폰을 천천히 좌우로 움직여요 → 흰 링이 보이면 탭하면 캐릭터가 나타나요.
- 다른 곳을 탭하면 캐릭터가 그 자리로 옮겨져요. 오른쪽 위 ✕를 누르면 끝나요.
- 바닥은 밝고 무늬가 있는 곳이 잘 잡혀요. 하얗고 반짝이는 바닥이나 유리는 잘 안 잡혀요.
- "AR 사용 불가"라고 나오면, 그 폰이나 브라우저가 AR을 지원하지 않는 거예요.

## 4. iPhone 연결하기 (Variant Launch)

1. https://launchar.app 에 가입해요 (무료 플랜).
2. 새 프로젝트를 만들어요.
3. 프로젝트 설정의 **허용 도메인(authorized domains)** 에 `내아이디.github.io`를 추가해요.
4. 프로젝트 화면 아래쪽에 있는 스크립트 주소에서 `key=` 뒤의 값을 복사해요.
   예) `https://launchar.app/sdk/v1?key=AbCd1234...&redirect=true` 에서 `AbCd1234...` 부분
5. `index.html` 위쪽 설정에 붙여 넣고 다시 올려요.
   ```js
   VARIANT_LAUNCH_KEY: 'AbCd1234...',
   ```
6. iPhone **Safari**로 주소를 열고 → **내 공간에서 보기** → 아래에 뜨는 창에서 **열기**를 누르면 돼요.

보는 사람들은 가입할 필요가 없어요. 무료 플랜에 로고 표시 같은 제한이 있는지는 가입할 때 확인해 주세요.

## 5. 내 Blender 캐릭터로 바꾸기

**Blender에서 내보낼 때**
- File → Export → **glTF 2.0** → Format: **glTF Binary (.glb)**
- Animation 항목을 체크하세요 (모든 동작이 같이 들어가요)
- 동작 이름: `Idle`(대기), `Wave`(손 흔들기), `Jump`(점프)
- 원점: 캐릭터 발바닥 중앙. 내보내기 전에 Apply → All Transforms를 하세요
- 용량: 5MB 이하를 권장해요 (크면 로딩이 느려요)

**올리기**
1. GitHub 저장소에서 **Add file → Upload files**를 눌러요. 파일 이름을 `models/character.glb`로 하고 싶으면, 업로드 화면에서 `models` 폴더째 끌어다 놓으면 돼요.
2. `index.html` 설정을 바꿔요.
   ```js
   MODEL_URL: 'models/character.glb',
   ```

**크기**
- `CHARACTER_HEIGHT: 0.3` → 키 30cm로 자동으로 맞춰져요 (모델 원래 크기와 상관없음)
- `CHARACTER_HEIGHT: null` → Blender에서 만든 크기 그대로 보여요 (1 = 1m)

## 6. 동작 추가하기

Blender에서 동작을 하나 더 만들고(예: `Dance`), `index.html`의 `ACTIONS`에 한 줄을 추가하면 버튼이 생겨요.

```js
ACTIONS: [
  { name: 'Wave',  label: '👋 손 흔들기', loop: false },
  { name: 'Jump',  label: '⬆️ 점프',     loop: false },
  { name: 'Dance', label: '💃 춤추기',   loop: true },
],
```

- `name`은 Blender 동작 이름과 철자·대소문자까지 같아야 해요.
- `loop: false`면 한 번 하고 대기 동작으로 돌아가요. `true`면 계속 반복해요.
- 샘플 로봇에는 `Dance`, `Walking`, `Running`, `ThumbsUp`, `Yes`, `No`, `Punch` 같은 동작도 들어 있어서 바로 시험해 볼 수 있어요.

## 문제가 생기면

- **"캐릭터 파일을 불러오지 못했어요"**: `MODEL_URL` 경로나 파일 이름의 대소문자를 확인하세요.
- **버튼을 눌러도 "동작이 모델에 없어요"**: 동작 이름이 다른 거예요. PC 브라우저로 주소를 열고 F12 → Console 탭을 보면 `이 모델에 들어 있는 동작:` 목록이 나와요.
- **고쳤는데 반영이 안 돼요**: 1~2분 기다린 뒤 새로고침하세요. 폰에서는 탭을 닫았다가 다시 열어 보세요.
