<div align="center">

<img src="assets/icon.svg" alt="TIDAL DOWNLOADER" width="160" />

# TIDAL DOWNLOADER

[English](README.md) | **한국어**

Tidal을 위한 고음질 데스크톱 클라이언트 — 무손실 **FLAC** 다운로드(16비트 / 24비트, 최대 192 kHz HI_RES_LOSSLESS), **플레이리스트** 통째로 받기, **비트퍼펙트** 재생(Windows: WASAPI exclusive 모드, macOS: Core Audio Hog Mode), 라이브러리 정리와 일괄 태그 편집까지.<br>Electron + React 기반, Windows / macOS 지원.

[![Release](https://img.shields.io/github/v/release/ARCLIGHTSTRVL/tidal-downloader?style=flat-square)](https://github.com/ARCLIGHTSTRVL/tidal-downloader/releases/latest)
[![Downloads](https://img.shields.io/github/downloads/ARCLIGHTSTRVL/tidal-downloader/total?style=flat-square)](https://github.com/ARCLIGHTSTRVL/tidal-downloader/releases)
![Platform](https://img.shields.io/badge/platform-Windows%20%7C%20macOS-lightgrey?style=flat-square)
[![License](https://img.shields.io/badge/license-Proprietary-blue?style=flat-square)](LICENSE)

</div>

> **최신 버전: v1.0.5** — 폴더·파일 이름 규칙, 앨범 유형별 분류, 라이브러리 정렬을 개선하고 설정 저장과 파일 작업 결과를 더 명확하게 안내합니다. **Windows x64와 macOS Apple Silicon / Intel**용 배포 파일을 제공합니다. [변경 기록](CHANGELOG.md)과 [릴리스 안내](https://github.com/ARCLIGHTSTRVL/tidal-downloader/releases/tag/v1.0.5)를 확인해 주세요.

---

## 스크린샷

일부 스크린샷은 이전 버전의 설정 화면입니다. 현재 이름 규칙 편집 방법은 [사용자 가이드](docs/USER_GUIDE.md#download)를 참고하세요.

<table>
  <tr>
    <td><img src="docs/images/library.png" alt="라이브러리 — 아티스트·앨범 그리드와 플레이리스트" /></td>
    <td><img src="docs/images/library-list.png" alt="라이브러리 리스트 뷰 — 트랙별 음질 (FLAC 96 kHz / 24비트)" /></td>
  </tr>
  <tr>
    <td><img src="docs/images/search-home.png" alt="검색 홈 — 라이브러리 통계와 즐겨찾기" /></td>
    <td><img src="docs/images/search.png" alt="검색 결과 — 앨범·플레이리스트·트랙" /></td>
  </tr>
  <tr>
    <td><img src="docs/images/album-download.png" alt="앨범 다운로드 진행 — 트랙 3개 동시" /></td>
    <td><img src="docs/images/album-art.png" alt="원본 해상도 앨범 아트 라이트박스" /></td>
  </tr>
  <tr>
    <td><img src="docs/images/now-playing.png" alt="재생 화면 — 앨범 배경 앰비언트" /></td>
    <td><img src="docs/images/album-info.png" alt="앨범 정보 오버레이 — Tidal 메타데이터와 트랙 목록" /></td>
  </tr>
  <tr>
    <td><img src="docs/images/exclusive-mode.png" alt="장치별 exclusive 모드(비트퍼펙트) 옵션" /></td>
    <td><img src="docs/images/tag-editor.png" alt="일괄 태그 편집기와 파일 정보" /></td>
  </tr>
  <tr>
    <td><img src="docs/images/downloads.png" alt="다운로드 패널 — 진행률과 완료 목록" /></td>
    <td><img src="docs/images/settings-korean.png" alt="한국어 설정 화면 (English/한국어 UI)" /></td>
  </tr>
</table>

## 동작 방식

- Tidal의 FLAC 스트림은 오디오 재인코딩 없이 표준 FLAC으로 저장합니다. Max는 최대 24비트 / 192 kHz 무손실을 요청하며, 실제 음질은 곡과 Tidal의 응답에 따라 달라집니다.
- HI_RES_LOSSLESS에 쓰이는 DASH 매니페스트는 세그먼트를 조립한 뒤 ffmpeg로 무손실 remux(`-c:a copy`)합니다.
- 독점 재생은 Windows의 **WASAPI exclusive 모드** 또는 macOS의 **Core Audio Hog Mode**로 음원의 샘플레이트를 요청합니다. 사용 가능한 형식과 독점 접근 여부는 오디오 장치에 따라 달라집니다.
- 지원하는 FLAC·M4A 다운로드에 Tidal 식별정보를 기록해 오프라인 인덱스 복구에 사용합니다(아래 참고). 태그·인덱스 기록이 완료되지 않으면 다운로드 화면에 안내합니다.
- Windows 설치 파일에는 ARCLIGHTSTRVL의 자체 서명과 타임스탬프가 있습니다. Windows의 신뢰 경고나 SmartScreen 경고가 나타날 수 있습니다.

## TIDAL 원본 정보 내장

이 앱은 **지원하는 음원 파일에 Tidal 원본 정보를 기록합니다** — 고유 식별자 `TIDAL_GUID`와 원본 기록 `TIDAL_META`(TIDAL 트랙 ID, 원본 제목·아티스트·앨범)를 FLAC은 Vorbis 코멘트로, M4A는 iTunes 태그로 저장합니다. 이 필드가 있는 파일은 파일명과 별개로 원본을 확인할 수 있습니다.

- **파일명이나 태그를 바꾼 뒤에도 원본을 확인할 수 있습니다.** 내장 식별정보를 보존하면 변경된 파일을 매칭하는 데 도움이 됩니다. 앱 밖에서 파일을 옮겼다면 현재 위치에서 라이브러리를 새로고침하거나 재구축해 주세요.
- **라이브러리 인덱스를 오프라인으로 복구합니다.** **재구축**은 접근 가능한 FLAC·M4A의 내장 식별정보를 읽습니다. 다른 태그 편집기를 사용할 때 이 필드를 보존해 주세요. 정보가 없거나 손상된 파일은 별도 확인이 필요할 수 있습니다.
- **다운로드 표시의 의미를 확인해 주세요.** 일부 구형 라이브러리 기록은 내장 식별정보가 필수가 아닐 때 제목으로 파일을 매칭합니다. 다운로드 ✓ 표시만으로 파일의 원본이 증명되는 것은 아닙니다.
- **평범한 표준 태그입니다.** 어떤 태그 편집기로도 읽고 지울 수 있는 일반 메타데이터 필드라서, 파일은 어디서든 재생되는 그냥 FLAC/M4A입니다.

## 기능

- **Tidal의 모든 음질 지원** — Max(HI_RES_LOSSLESS 24비트 최대 192 kHz FLAC), HiFi(16/44.1 FLAC), High(AAC 320 kbps, 정식 `.m4a`) — 전부 동일한 원본 정보 내장·라이브러리 기능 지원, 재인코딩 없음
- **최고 음질 DASH 지원** — HI_RES_LOSSLESS(24비트 / 96 kHz / 192 kHz) 세그먼트 조립 + ffmpeg remux
- **플레이리스트** — Tidal 플레이리스트 탐색(검색 결과, 내 플레이리스트+즐겨찾기, 최근 열람), 링크나 UUID를 붙여넣어 바로 열기, `playlists/<이름>/` 전용 폴더로 일괄 다운로드(플레이리스트 순번 파일명 + 커버 아트), 라이브러리의 전용 그룹으로 관리
- **비트퍼펙트 출력** — Windows: WASAPI exclusive 모드 + 네이티브 샘플레이트 협상 + 볼륨 고정 옵션. macOS: Core Audio Hog Mode + 노미널 샘플레이트 매칭
- **빠른 앨범 다운로드** — 앨범 트랙 3개 동시 다운로드, Max 음질 DASH는 트랙별 세그먼트 병렬
- **라이브러리** — 리스트/그리드 보기, 앨범명·연도·최근 추가순 정렬, 라이브러리 전체 이어듣기, 현재 곡 하이라이트, 플레이리스트 포함 검색
- **폴더·파일 이름 규칙** — 같은 규칙을 직접 입력하거나 태그로 조합하고, 프리셋과 예시 미리보기를 확인한 뒤 명시적으로 저장합니다. 새 설정의 기본값은 `앨범 아티스트/앨범` 폴더와 `트랙 번호 - 제목` 파일명이며, 기존의 유효한 저장값은 유지합니다
- **앨범 유형별 폴더** — `Albums`, `EPs`, `Singles`, `Compilations`로 분류할 수 있습니다. 기존 라이브러리는 변경 내용을 미리 본 뒤 **적용**을 눌러 정리합니다
- **태그 편집기** — 앨범 단위 일괄 편집, 앨범 아트 내장, 드래그-드롭 파일/폴더 가져오기, 다중 루트 새로고침
- **검색 & 탐색** — 아티스트/앨범 검색과 디스코그래피(앨범 / EP & 싱글), 즐겨찾기, 최근 기록, 검색 홈의 라이브러리 통계
- **재생** — 전용 `local://` 프로토콜 기반 로컬 재생, 셔플/반복(끄기 → 한 곡 → 앨범), 반응형 시크 바
- **앨범 아트** — 내장 품질 선택(320 / 640 / 1280), 호버 틸트, 원본 해상도 라이트박스, 아트 전용 저장 경로
- **업데이트 확인** (Windows + macOS) — 백그라운드 및 설정에서 새 버전 확인. 현재 서명 방식에서는 아래 배포 파일로 수동 업데이트해 주세요
- **English / 한국어** — 설정에서 UI 언어 즉시 전환
- **영속 상태** — 라이브러리 인덱스에 Tidal 정식 ID를 저장해 *LiSA*와 *LISA* 같은 동명 아티스트를 구분합니다. 파일은 가능한 경우 내장 식별정보로 매칭하며, 일부 구형 기록에는 제목 기반 매칭도 사용합니다
- **히스토리 내비게이션** — 마우스 엄지 버튼(XButton1 / XButton2)으로 앱 전체 뒤로/앞으로
- **음질 프로브** — 내 구독 티어가 실제로 무손실을 주는지 샘플 트랙으로 빠르게 확인(설정 → 사용 가능 음질 확인)
- **라이브러리 유지보수** — Tidal 온라인 기준 메타데이터 재동기화 + 파일 재정리, 또는 파일에 내장된 식별 태그(FLAC·M4A)만으로 인덱스 오프라인 재구축(Rebuild)

## 다운로드

### v1.0.5 (Windows + macOS)

[Releases](../../releases/latest)에서 받을 수 있습니다.

| 플랫폼 | 다운로드 |
|--------|------------|
| Windows 10/11 (x64) | [설치 파일 .exe](https://github.com/ARCLIGHTSTRVL/tidal-downloader/releases/download/v1.0.5/TIDAL-DOWNLOADER-Setup-1.0.5.exe) |
| macOS 12+ Apple Silicon (arm64) | [DMG](https://github.com/ARCLIGHTSTRVL/tidal-downloader/releases/download/v1.0.5/TIDAL-DOWNLOADER-1.0.5-arm64.dmg) · [ZIP](https://github.com/ARCLIGHTSTRVL/tidal-downloader/releases/download/v1.0.5/TIDAL-DOWNLOADER-1.0.5-arm64-mac.zip) |
| macOS 12+ Intel (x64) | [DMG](https://github.com/ARCLIGHTSTRVL/tidal-downloader/releases/download/v1.0.5/TIDAL-DOWNLOADER-1.0.5.dmg) · [ZIP](https://github.com/ARCLIGHTSTRVL/tidal-downloader/releases/download/v1.0.5/TIDAL-DOWNLOADER-1.0.5-mac.zip) |

Intel용 빌드는 Apple Silicon의 Rosetta에서 실행을 확인했으며, Intel Mac 실기 검증은 하지 않았습니다. Mac 배포본은 Apple Developer ID 서명·공증을 거치지 않아 Gatekeeper 기본 검사에서 차단됩니다.

## 설치

### Windows
1. TIDAL DOWNLOADER를 종료하고 위 릴리스에서 `TIDAL-DOWNLOADER-Setup-1.0.5.exe`를 받습니다.
2. 설치 파일을 실행합니다. 자체 서명 인증서가 Windows의 신뢰 루트에 포함되지 않아 신뢰 경고나 SmartScreen 경고가 나타날 수 있습니다.
3. 설치 마법사를 따라갑니다. 사용자 단위 설치는 보통 관리자 권한이 필요하지 않습니다. 모든 사용자용 설치나 보호된 기존 설치 경로를 변경할 때는 관리자 권한을 요청할 수 있습니다.

### macOS
1. CPU에 맞는 `.dmg`를 받습니다(Apple Silicon은 `arm64`, Intel은 `x64`).
2. 마운트한 뒤 *TIDAL DOWNLOADER*를 `/Applications`로 끌어 넣습니다.
3. macOS가 차단하면 **시스템 설정 → 개인정보 보호 및 보안**(Monterey에서는 **시스템 환경설정 → 보안 및 개인 정보 보호**)을 확인하고, 실행을 허용하려면 **그래도 열기**를 선택합니다.

### 이전 버전에서 업데이트

운영체제에 맞는 배포 파일을 받아 수동으로 업데이트해 주세요. 트레이나 메뉴 막대에 남은 앱까지 종료한 뒤 진행합니다. 현재 Windows·Mac 서명 방식에서는 자동 설치가 원활하지 않을 수 있습니다.

다운로드 경로와 이름 규칙 등 기존의 유효한 저장 설정은 유지합니다. 새 기본 이름 규칙은 유효한 저장값이 없을 때 적용되며, 버전 업데이트만으로 기존 음악 파일을 재정리하지 않습니다. 규칙을 바꾸려면 설정에서 먼저 저장하고, 기존 라이브러리 변경은 미리보기 후 명시적으로 적용해 주세요.

## 요구 사항

- 요청한 음질을 이용할 수 있는 **활성 Tidal 구독**이 필요합니다. 계정·지역·곡에 따라 제공 음질이 달라질 수 있습니다.
- **Windows 10/11 (x64)** 또는 **macOS 12 Monterey 이상**(Intel / Apple Silicon).
- 앱, 음악 라이브러리와 다운로드 임시 파일을 저장할 여유 공간이 필요합니다.

## 빠른 시작

1. 앱을 실행하고 **로그인**을 누른 뒤 Tidal 로그인 창에서 인증을 완료합니다.
2. **설정 → 다운로드 위치**에서 다운로드 폴더를 지정합니다. 앨범 아트 폴더는 `<다운로드 경로>/art`로 자동 설정됩니다.
3. 아티스트나 앨범을 검색하거나 플레이리스트 링크를 붙여넣고 — 트랙의 **다운로드**, 앨범의 **전체 다운로드**, 또는 플레이리스트 일괄 다운로드를 누릅니다.
4. **라이브러리** 탭에서 받은 음악을 재생합니다 — 리스트 모드로 훑고, 그리드 모드로 시각적으로 탐색하고, 전용 플레이리스트 그룹으로 관리합니다.
5. **설정 → 다운로드 이름 규칙**에서 폴더·파일 규칙을 정한 뒤 **저장**합니다. 메타데이터 일괄 수정은 **태그 편집기**에서 할 수 있습니다.

## 음질

설정에서 **Max**, **HiFi**, **High** 중 하나를 선택합니다. **사용 가능한 음질 확인**으로 현재 계정에 Tidal이 어떤 음질을 제공하는지 확인할 수 있습니다. High는 원래 AAC이며, 무손실 요청에 AAC가 반환된 경우와 구분해 표시합니다.

Max·HiFi 다운로드는 표준 FLAC으로 저장됩니다(MP4 래퍼 없음). Tidal 매니페스트가 DASH(HI_RES_LOSSLESS)면 세그먼트를 조립해 ffmpeg로 무손실 remux(`-c:a copy`)합니다. High 티어는 진짜 AAC 320 kbps를 `.m4a`로 저장합니다. 어떤 경우에도 재인코딩·위장은 없습니다 — 무손실 티어가 제공되지 않는 트랙은 자연스럽게 폴백하며, 재인코딩된 AAC를 FLAC으로 속여 저장하지 않습니다.

오디오 장치 선택기의 **exclusive 모드 사용**을 켜면 장치를 독점 점유해 음원의 샘플레이트/비트 심도(16/44.1, 24/96, 24/192)에 맞춰 비트퍼펙트로 출력합니다 — Windows는 WASAPI exclusive 모드, macOS는 Core Audio Hog Mode.

## 버그 신고

버그나 기능 요청은 [Issues 페이지](../../issues)에 올려주세요.

버그 신고 시 포함해 주세요:
- 앱 버전(**설정** 페이지 하단)
- OS와 버전
- 재현 방법
- 요청한 음질(Max / HiFi / High), 필요하면 구독 요금제
- 재현 가능하면 콘솔 출력 — Windows는 PowerShell에서:
  ```powershell
  $env:ELECTRON_ENABLE_LOGGING=1
  & "$env:LOCALAPPDATA\Programs\tidal-downloader\TIDAL DOWNLOADER.exe"
  ```

## 면책 조항

이 앱은 비공식 서드파티 도구이며, Tidal 또는 Aspiro AB와 **아무런 제휴·후원·보증 관계가 없습니다**.

Tidal 서비스 약관 준수는 사용자 본인의 책임입니다. 다운로드는 구독으로 이미 비용을 지불한 음악을 개인적, 오프라인으로 듣기 위한 용도입니다. **다운로드한 콘텐츠를 재배포하지 마세요.**

내부적으로 FFmpeg를 사용합니다. 적용되는 LGPL 또는 GPL 조건은 바이너리의 빌드 옵션에 따라 달라지며, 포함된 Windows 빌드에는 GPL 구성요소가 활성화되어 있습니다. [FFmpeg 라이선스 안내](https://ffmpeg.org/legal.html), [FFmpeg 소스 다운로드](https://ffmpeg.org/download.html), [`ffmpeg-static` 바이너리 릴리스와 라이선스 파일](https://github.com/eugeneware/ffmpeg-static/releases/tag/b6.1.1)을 참고하세요.

## 라이선스

Copyright © 2026 **ARCLIGHTSTRVL**. All rights reserved.

컴파일된 애플리케이션은 개인 사용 목적으로 있는 그대로 제공됩니다. 소스 코드는 공개되지 않습니다. 전체 조건은 [LICENSE](LICENSE)를 참고하세요.

## 후원

이곳의 [GitHub 저장소](https://github.com/ARCLIGHTSTRVL/tidal-downloader)나 앱 설정의 **업데이트 확인** 아래 **GitHub Star** 링크에서 별을 남길 수 있습니다.

TIDAL DOWNLOADER가 유용했다면 [Ko-fi](https://ko-fi.com/arclights)에서 개발을 후원할 수 있습니다. 모든 후원이 프로젝트 유지에 큰 힘이 됩니다 — 감사합니다.

[![Ko-fi](https://img.shields.io/badge/Support_on-Ko--fi-FF5E5B?style=flat-square&logo=ko-fi&logoColor=white)](https://ko-fi.com/arclights)

---

Built by **ARCLIGHTSTRVL**.
