# 왕도 탈출 (가제) — Android APK 프로젝트 틀

루트 무브 SRPG 전투 프로토타입을 Capacitor로 감싸 APK로 만드는 프로젝트입니다. 게임 본체는 `www/index.html` 한 파일이고, 폰 브라우저에서 그냥 열어도 그대로 돌아갑니다.

## 들어 있는 것

| 파일 | 내용 |
| --- | --- |
| `www/index.html` | 게임 전체: 규칙 엔진, AI, 터치 UI |
| `capacitor.config.json` | 앱 이름, 패키지명, 배경색 |
| `package.json` | Capacitor 의존성과 빌드 스크립트 |

## 준비물

- Node.js 20 이상
- Android Studio (Android SDK와 JDK가 같이 설치됨)

## APK 만들기

1. 이 폴더에서 `npm install`
2. `npx cap add android` — `android/` 폴더가 생깁니다
3. 세로 고정: `android/app/src/main/AndroidManifest.xml`의 `<activity ...>` 태그에 `android:screenOrientation="portrait"` 추가
4. `npx cap sync android`
5. 둘 중 하나로 빌드
   - `npx cap open android` 후 Android Studio에서 **Build → Build Bundle(s) / APK(s) → Build APK(s)**
   - 또는 `npm run apk` → `android/app/build/outputs/apk/debug/app-debug.apk`

코드를 고친 뒤에는 `www/index.html`만 수정하고 `npx cap sync android` 후 다시 빌드하면 됩니다.

## 코드 구조

`//#CORE-START` 와 `//#CORE-END` 사이가 화면과 분리된 규칙·데이터·AI입니다. 수치 조정은 여기만 보면 됩니다.

| 이름 | 내용 |
| --- | --- |
| `CFG` | 치명타 배율, 가드 감소율, 반격·가드 상한, 마나 회복, 창 보너스, 분노 획득량, 경험치 |
| `WEAPONS` | 무기군별 위력 계수, 기본 공격횟수, 치명타, 사거리 |
| `SKILLS`, `SPECIALS` | 마법 스킬, 분노 필살기 |
| `GEAR` | 장비 목록(무기·방어구·장신구) |
| `ROSTER` | 캐릭터 능력치, 성장률, 시작 장비, 필살기, 드롭 |
| `MAP` | 1화 맵 (`#` 벽, `E` 탈출 지점) |

## 알아둘 점

- 패키지명 `com.example.royalescape`는 임시입니다. 스토어에 올리기 전에 바꾸세요.
- 글꼴은 Google Fonts에서 불러오므로 오프라인일 때는 기본 글꼴로 보입니다. 정식 빌드에서는 글꼴 파일을 `www/`에 넣어 로컬로 불러오면 됩니다.
- 아직 세이브가 없습니다. 다음 단계에서 `@capacitor/preferences`로 회차·레벨·장비를 저장할 예정입니다.
