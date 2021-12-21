---
title: pip install 중 ERROR: Could not install packages due to an OSError: [Errno 2] No such file or directory
---

ERROR: Could not install packages due to an OSError: [Errno 2] No such file or directory

파이썬을 Visual Studio Code 등을 통해 설치를 하게 되면 패키지가 설치되는 경로가 C:\Users\\(...)\AppData\Roaming\Python\... 과 같이 매우 길어지고, 이렇게 되면 경로 길이가 윈도우가 허용하는 한계인 256B를 넘어서게 되어 위와 같은 에러 메시지가 나타날 수 있습니다.

레지스트리 설정을 변경하여 문제를 해결할 수 있습니다. cmd 창에서 regedit을 쳐서 레지스트리 편집기를 실행 후, 

```html
HKEY_LOCAL_MACHINE > SYSTEM > CurrentControlSet > Control > FileSystem
```

위 경로로 들어가 LongPathsEnabled를 더블클릭 후 값 데이터(V)를 1로 변경 후 확인을 누른 후 다시 설치해보면 문제가 해결됩니다.

