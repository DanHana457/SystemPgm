# 1. pwd

## 기능

사용자가 현재 머무르고 있는 작업 디렉터리의 전체 절대 경로를 출력합니다.

```c
#include <stdio.h>
#include <unistd.h>
#include <limits.h>

int main() {
    char cwd[PATH_MAX];

    if (getcwd(cwd, sizeof(cwd)) != NULL) {
        printf("%s\n", cwd);
    } else {
        perror("pwd 오류");
        return 1;
    }

    return 0;
}
```

# 2. ls

## 기능

현재 디렉터리에 존재하는 파일과 폴더 목록을 출력합니다.

```c
#include <stdio.h>
#include <dirent.h>

int main() {
    DIR *dir;
    struct dirent *entry;

    dir = opendir(".");

    if (dir == NULL) {
        perror("ls 오류");
        return 1;
    }

    while ((entry = readdir(dir)) != NULL) {
        printf("%s\n", entry->d_name);
    }

    closedir(dir);

    return 0;
}
```

# 3. mkdir

## 기능

새로운 디렉터리를 생성합니다.

```c
#include <stdio.h>
#include <sys/stat.h>

int main() {

    if (mkdir("new_folder", 0755) == 0) {
        printf("디렉터리 생성 완료\n");
    } else {
        perror("mkdir 오류");
    }

    return 0;
}
```

# 4. rmdir

## 기능

비어 있는 디렉터리를 삭제합니다.

```c
#include <stdio.h>
#include <unistd.h>

int main() {

    if (rmdir("new_folder") == 0) {
        printf("디렉터리 삭제 완료\n");
    } else {
        perror("rmdir 오류");
    }

    return 0;
}
```

# 5. touch

## 기능

빈 파일을 생성하거나 파일의 수정 시간을 갱신합니다.

```c
#include <stdio.h>

int main() {

    FILE *fp = fopen("test.txt", "a");

    if (fp == NULL) {
        perror("touch 오류");
        return 1;
    }

    fclose(fp);

    printf("파일 생성 완료\n");

    return 0;
}
```

# 6. cp

## 기능

파일을 복사합니다.

```c
#include <stdio.h>

int main() {

    FILE *src = fopen("source.txt", "r");
    FILE *dst = fopen("copy.txt", "w");

    char ch;

    if (!src || !dst) {
        perror("cp 오류");
        return 1;
    }

    while ((ch = fgetc(src)) != EOF) {
        fputc(ch, dst);
    }

    fclose(src);
    fclose(dst);

    printf("복사 완료\n");

    return 0;
}
```

# 7. mv

## 기능

파일 이름을 변경하거나 파일을 이동합니다.

```c
#include <stdio.h>

int main() {

    if (rename("old.txt", "new.txt") == 0) {
        printf("이동 완료\n");
    } else {
        perror("mv 오류");
    }

    return 0;
}
```

# 8. rm

## 기능

파일을 삭제합니다.

```c
#include <stdio.h>
#include <unistd.h>

int main() {

    if (remove("test.txt") == 0) {
        printf("삭제 완료\n");
    } else {
        perror("rm 오류");
    }

    return 0;
}
```

# 9. cat

## 기능

파일 내용을 화면에 출력합니다.

```c
#include <stdio.h>

int main() {

    FILE *fp = fopen("test.txt", "r");

    char ch;

    if (!fp) {
        perror("cat 오류");
        return 1;
    }

    while ((ch = fgetc(fp)) != EOF) {
        putchar(ch);
    }

    fclose(fp);

    return 0;
}
```

# 10. head

## 기능

파일의 앞부분을 출력합니다.

```c
#include <stdio.h>

int main() {

    FILE *fp = fopen("test.txt", "r");

    char line[256];
    int count = 0;

    while (fgets(line, sizeof(line), fp) && count < 10) {
        printf("%s", line);
        count++;
    }

    fclose(fp);

    return 0;
}
```
# 11. tail

## 기능

파일의 마지막 부분을 출력합니다.

```c id="wsmcly"
#include <stdio.h>
#include <stdlib.h>

#define MAX_LINES 10
#define MAX_LEN 256

int main() {
    FILE *fp = fopen("test.txt", "r");

    if (!fp) {
        perror("tail 오류");
        return 1;
    }

    char lines[MAX_LINES][MAX_LEN];
    int count = 0;

    while (fgets(lines[count % MAX_LINES], MAX_LEN, fp)) {
        count++;
    }

    int start = count > MAX_LINES ? count - MAX_LINES : 0;

    for (int i = start; i < count; i++) {
        printf("%s", lines[i % MAX_LINES]);
    }

    fclose(fp);

    return 0;
}
```

# 12. grep

## 기능

파일에서 특정 문자열을 검색합니다.

```c id="48hfgf"
#include <stdio.h>
#include <string.h>

int main() {

    FILE *fp = fopen("test.txt", "r");

    char line[256];

    if (!fp) {
        perror("grep 오류");
        return 1;
    }

    while (fgets(line, sizeof(line), fp)) {
        if (strstr(line, "hello")) {
            printf("%s", line);
        }
    }

    fclose(fp);

    return 0;
}
```

# 13. find

## 기능

특정 파일을 검색합니다.

```c id="uk30cl"
#include <stdio.h>
#include <dirent.h>
#include <string.h>

int main() {

    DIR *dir;
    struct dirent *entry;

    dir = opendir(".");

    if (!dir) {
        perror("find 오류");
        return 1;
    }

    while ((entry = readdir(dir)) != NULL) {

        if (strcmp(entry->d_name, "test.txt") == 0) {
            printf("파일 발견 : %s\n", entry->d_name);
        }
    }

    closedir(dir);

    return 0;
}
```

# 14. chmod

## 기능

파일 권한을 변경합니다.

```c id="5ngn7a"
#include <stdio.h>
#include <sys/stat.h>

int main() {

    if (chmod("test.txt", 0777) == 0) {
        printf("권한 변경 완료\n");
    } else {
        perror("chmod 오류");
    }

    return 0;
}
```

# 15. chown

## 기능

파일 소유자를 변경합니다.

```c id="7q9s8w"
#include <stdio.h>
#include <unistd.h>

int main() {

    if (chown("test.txt", 1000, 1000) == 0) {
        printf("소유자 변경 완료\n");
    } else {
        perror("chown 오류");
    }

    return 0;
}
```

# 16. ps

## 기능

현재 실행 중인 프로세스 정보를 출력합니다.

```c id="mjlwmn"
#include <stdio.h>
#include <stdlib.h>

int main() {

    system("ps");

    return 0;
}
```

# 17. top

## 기능

실시간 프로세스 상태를 확인합니다.

```c id="fj1mca"
#include <stdio.h>
#include <stdlib.h>

int main() {

    system("top");

    return 0;
}
```

# 18. kill

## 기능

지정한 프로세스를 종료합니다.

```c id="jw7s7m"
#include <stdio.h>
#include <signal.h>

int main() {

    int pid = 1234;

    if (kill(pid, SIGKILL) == 0) {
        printf("프로세스 종료 완료\n");
    } else {
        perror("kill 오류");
    }

    return 0;
}
```

# 19. df

## 기능

디스크 사용량 정보를 출력합니다.

```c id="04kmec"
#include <stdio.h>
#include <stdlib.h>

int main() {

    system("df -h");

    return 0;
}
```

# 20. du

## 기능

디렉터리의 용량을 출력합니다.

```c id="hvl8c8"
#include <stdio.h>
#include <stdlib.h>

int main() {

    system("du -sh .");

    return 0;
}
```
# 21. free

## 기능

시스템의 메모리 사용량을 출력합니다.

```c id="h8akdf"
#include <stdio.h>
#include <stdlib.h>

int main() {

    system("free -h");

    return 0;
}
```

# 22. uname

## 기능

운영체제 및 커널 정보를 출력합니다.

```c id="xj7hda"
#include <stdio.h>
#include <stdlib.h>

int main() {

    system("uname -a");

    return 0;
}
```

# 23. ping

## 기능

네트워크 연결 상태를 확인합니다.

```c id="p4md7v"
#include <stdio.h>
#include <stdlib.h>

int main() {

    system("ping -c 4 google.com");

    return 0;
}
```

# 24. wget

## 기능

인터넷에서 파일을 다운로드합니다.

```c id="w6zv91"
#include <stdio.h>
#include <stdlib.h>

int main() {

    system("wget https://example.com/file.txt");

    return 0;
}
```

# 25. tar

## 기능

파일 및 디렉터리를 압축하거나 압축을 해제합니다.

```c id="8q4scl"
#include <stdio.h>
#include <stdlib.h>

int main() {

    system("tar -cvf archive.tar test.txt");

    return 0;
}
```

# 26. zip

## 기능

파일을 ZIP 형식으로 압축합니다.

```c id="y5c9dr"
#include <stdio.h>
#include <stdlib.h>

int main() {

    system("zip archive.zip test.txt");

    return 0;
}
```

# 27. unzip

## 기능

ZIP 압축 파일을 해제합니다.

```c id="j4r9qa"
#include <stdio.h>
#include <stdlib.h>

int main() {

    system("unzip archive.zip");

    return 0;
}
```

# 28. whoami

## 기능

현재 로그인한 사용자 이름을 출력합니다.

```c id="g2w7hs"
#include <stdio.h>
#include <stdlib.h>

int main() {

    system("whoami");

    return 0;
}
```

# 29. hostname

## 기능

현재 시스템의 호스트 이름을 출력합니다.

```c id="m9v2tp"
#include <stdio.h>
#include <stdlib.h>

int main() {

    system("hostname");

    return 0;
}
```

# 30. date

## 기능

현재 날짜와 시간을 출력합니다.

```c id="t8p5ke"
#include <stdio.h>
#include <stdlib.h>

int main() {

    system("date");

    return 0;
}
```
