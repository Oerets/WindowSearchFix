# FixWindowsSearch.exe

윈도우 11 검색창 느려짐 현상을 해결하는 도구입니다.

[![Download](https://img.shields.io/github/v/release/Oerets/WindowSearchFix?label=Download&logo=github)](https://github.com/Oerets/WindowSearchFix/releases/latest/download/FixWindowsSearch.exe)

> 출처: [Windows 11 search bar has recently been very slow - r/WindowsHelp](https://www.reddit.com/r/WindowsHelp/comments/1jht1up/windows_11_search_bar_has_recently_been_very_slow/?tl=ko)

---

## 원인

윈도우 누적 업데이트 이후 아래 두 패키지가 오작동하면서 검색창이 느려집니다.

| 패키지 | 역할 |
|---|---|
| `MicrosoftWindows.Client.WebExperience` | 검색창의 위젯 및 웹 검색 담당 |
| `MicrosoftWindows.Client.CBS` | `SearchHost.exe` 담당 (로컬 검색) |

마이크로소프트가 Copilot+ PC를 위한 시맨틱 검색 통합 등 검색/인덱싱에 대규모 변경을 가하면서 부작용이 발생한 것으로 추정됩니다.

---

## 실행 방법

1. `FixWindowsSearch.exe` 더블클릭
2. UAC(사용자 계정 컨트롤) 창에서 **예** 클릭
3. 자동으로 초기화 진행 후 완료 메시지 확인

> 관리자 권한이 필요합니다.

---

## 내부 동작

실행 시 아래 PowerShell 명령어를 순서대로 수행합니다.

```powershell
Get-AppxPackage *MicrosoftWindows.Client.WebExperience* | Reset-AppxPackage
Get-AppxPackage *MicrosoftWindows.Client.CBS* | Reset-AppxPackage
```

`Reset-AppxPackage`는 앱을 삭제하지 않고 초기 상태로 재설정합니다.

---

## 그래도 안 될 때 — 추가 해결 방법

### 방법 1. 검색 인덱스 다시 빌드

1. 시작 메뉴에서 **인덱싱 옵션** 검색
2. **고급 옵션** 클릭
3. **다시 빌드** 버튼 클릭

### 방법 2. 인덱스 파일 직접 삭제

1. 서비스(`services.msc`)에서 **Windows Search** 서비스 중지
2. 아래 폴더의 파일 전체 삭제
   ```
   C:\ProgramData\Microsoft\Search\Data\Applications\Windows\GatherLogs\SystemIndex
   ```
3. Windows Search 서비스 다시 시작

### 방법 3. 웹 검색 결과 비활성화

검색창 설정에서 웹 검색 결과를 끄면 로딩이 빨라집니다.

### 방법 4. "시작하기" 앱 초기화

1. 시작 메뉴에서 **시작하기** 검색
2. 우클릭 → **앱 설정** → **데이터 초기화**

### 최후의 수단 — ISO 인플레이스 업그레이드

위 방법이 모두 실패한 경우, 윈도우 11 ISO로 인플레이스 업그레이드를 수행합니다.
`setup.exe` 실행 시 데이터와 앱을 유지한 채로 윈도우만 재설치됩니다.

---

## 재발 시

윈도우 업데이트 후 증상이 재발하는 경우가 있습니다. 이 경우 `FixWindowsSearch.exe`를 다시 실행하거나, 검색 인덱스를 다시 빌드하세요.
