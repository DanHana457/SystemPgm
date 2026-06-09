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

# 5. rm

## 기능

파일을 삭제합니다.

```c
#include <stdio.h>

int main() {

    if (remove("test.txt") == 0) {
        printf("삭제 완료\n");
    } else {
        perror("rm 오류");
    }

    return 0;
}
```

# 6. mv

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

# 7. cp

## 기능

파일을 복사합니다.

```c
#include <stdio.h>

int main() {

    FILE *src = fopen("source.txt", "r");
    FILE *dst = fopen("copy.txt", "w");

    int ch;

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

# 8. touch

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

# 9. head

## 기능

파일의 앞부분 10줄을 출력합니다.

```c
#include <stdio.h>

int main() {

    FILE *fp = fopen("test.txt", "r");

    char line[256];
    int count = 0;

    if (fp == NULL) {
        perror("head 오류");
        return 1;
    }

    while (fgets(line, sizeof(line), fp) && count < 10) {
        printf("%s", line);
        count++;
    }

    fclose(fp);

    return 0;
}
```

# 10. tail

## 기능

파일의 마지막 10줄을 출력합니다.

```c
#include <stdio.h>

#define MAX_LINES 10
#define MAX_LEN 256

int main() {

    FILE *fp = fopen("test.txt", "r");

    if (fp == NULL) {
        perror("tail 오류");
        return 1;
    }

    char lines[MAX_LINES][MAX_LEN];
    int count = 0;

    while (fgets(lines[count % MAX_LINES], MAX_LEN, fp)) {
        count++;
    }

    int start = (count > MAX_LINES) ? count - MAX_LINES : 0;

    for (int i = start; i < count; i++) {
        printf("%s", lines[i % MAX_LINES]);
    }

    fclose(fp);

    return 0;
}
```
# 11. cat

## 기능

파일의 내용을 화면에 출력합니다.

```c id="gv7i4u"
#include <stdio.h>

int main() {

    FILE *fp = fopen("test.txt", "r");
    int ch;

    if (fp == NULL) {
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

# 12. find

## 기능

현재 디렉터리에서 특정 파일을 검색합니다.

```c id="g31l5h"
#include <stdio.h>
#include <dirent.h>
#include <string.h>

int main() {

    DIR *dir;
    struct dirent *entry;

    dir = opendir(".");

    if (dir == NULL) {
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

# 13. whoami

## 기능

현재 로그인한 사용자의 이름을 출력합니다.

```c id="43x4ow"
#include <stdio.h>
#include <unistd.h>
#include <pwd.h>

int main() {

    struct passwd *pw;

    pw = getpwuid(getuid());

    if (pw == NULL) {
        perror("whoami 오류");
        return 1;
    }

    printf("%s\n", pw->pw_name);

    return 0;
}
```

# 14. hostname

## 기능

현재 시스템의 호스트 이름을 출력합니다.

```c id="dz3r8u"
#include <stdio.h>
#include <unistd.h>

int main() {

    char hostname[256];

    if (gethostname(hostname, sizeof(hostname)) == 0) {
        printf("%s\n", hostname);
    } else {
        perror("hostname 오류");
    }

    return 0;
}
```

# 15. wc

## 기능

파일의 줄 수, 단어 수, 문자 수를 출력합니다.

```c id="0g8lvh"
#include <stdio.h>
#include <ctype.h>

int main() {

    FILE *fp = fopen("test.txt", "r");

    if (fp == NULL) {
        perror("wc 오류");
        return 1;
    }

    int ch;
    int lines = 0;
    int words = 0;
    int chars = 0;
    int inWord = 0;

    while ((ch = fgetc(fp)) != EOF) {

        chars++;

        if (ch == '\n')
            lines++;

        if (isspace(ch)) {
            inWord = 0;
        } else if (!inWord) {
            words++;
            inWord = 1;
        }
    }

    printf("줄 수 : %d\n", lines);
    printf("단어 수 : %d\n", words);
    printf("문자 수 : %d\n", chars);

    fclose(fp);

    return 0;
}
```

# 16. date

## 기능

현재 날짜와 시간을 출력합니다.

```c id="e1rq4r"
#include <stdio.h>
#include <time.h>

int main() {

    time_t now;
    time(&now);

    printf("%s", ctime(&now));

    return 0;
}
```

# 17. echo

## 기능

입력한 문자열을 화면에 출력합니다.

```c id="y0gkwh"
#include <stdio.h>

int main() {

    char str[100];

    printf("문자열 입력 : ");
    fgets(str, sizeof(str), stdin);

    printf("%s", str);

    return 0;
}
```

# 18. cd

## 기능

현재 작업 디렉터리를 변경합니다.

```c id="f5r2y9"
#include <stdio.h>
#include <unistd.h>

int main() {

    if (chdir("/tmp") == 0) {
        printf("디렉터리 변경 완료\n");
    } else {
        perror("cd 오류");
    }

    return 0;
}
```

# 19. chmod

## 기능

파일의 접근 권한을 변경합니다.

```c id="9jw77f"
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

# 20. chown

## 기능

파일의 소유자를 변경합니다.

```c id="0vc9y5"
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
# 21. kill

## 기능

지정한 프로세스에 종료 시그널을 보내 프로세스를 종료합니다.

```c id="4zkjvz"
#include <stdio.h>
#include <signal.h>

int main() {

    int pid;

    printf("종료할 PID 입력 : ");
    scanf("%d", &pid);

    if (kill(pid, SIGTERM) == 0) {
        printf("프로세스 종료 성공\n");
    } else {
        perror("kill 오류");
    }

    return 0;
}
```

# 22. sleep

## 기능

지정한 시간(초) 동안 프로그램 실행을 중지합니다.

```c id="9sltl4"
#include <stdio.h>
#include <unistd.h>

int main() {

    printf("5초 동안 대기합니다.\n");

    sleep(5);

    printf("대기 종료\n");

    return 0;
}
```

# 23. uptime

## 기능

시스템이 부팅된 후 경과된 시간을 출력합니다.

```c id="4hzn4w"
#include <stdio.h>
#include <sys/sysinfo.h>

int main() {

    struct sysinfo info;

    if (sysinfo(&info) == 0) {

        long uptime = info.uptime;

        printf("가동 시간 : %ld초\n", uptime);

    } else {
        perror("uptime 오류");
    }

    return 0;
}
```

# 24. ps

## 기능

현재 실행 중인 프로세스 정보를 출력합니다.

```c id="zy92kk"
#include <stdio.h>
#include <dirent.h>
#include <ctype.h>

int main() {

    DIR *dir;
    struct dirent *entry;

    dir = opendir("/proc");

    if (dir == NULL) {
        perror("ps 오류");
        return 1;
    }

    printf("PID 목록\n");

    while ((entry = readdir(dir)) != NULL) {

        if (isdigit(entry->d_name[0])) {
            printf("%s\n", entry->d_name);
        }
    }

    closedir(dir);

    return 0;
}
```

# 25. df

## 기능

파일 시스템의 전체 용량과 사용 가능한 용량을 출력합니다.

```c id="35dq6p"
#include <stdio.h>
#include <sys/statvfs.h>

int main() {

    struct statvfs fs;

    if (statvfs("/", &fs) == 0) {

        unsigned long total =
            (fs.f_blocks * fs.f_frsize) / (1024 * 1024);

        unsigned long free =
            (fs.f_bfree * fs.f_frsize) / (1024 * 1024);

        printf("전체 용량 : %lu MB\n", total);
        printf("남은 용량 : %lu MB\n", free);

    } else {
        perror("df 오류");
    }

    return 0;
}
```

# 26. du

## 기능

파일의 크기를 출력합니다.

```c id="3z9b4i"
#include <stdio.h>
#include <sys/stat.h>

int main() {

    struct stat st;

    if (stat("test.txt", &st) == 0) {

        printf("파일 크기 : %ld 바이트\n",
               st.st_size);

    } else {
        perror("du 오류");
    }

    return 0;
}
```

# 27. sort

## 기능

문자열을 오름차순으로 정렬합니다.

```c id="x2yq74"
#include <stdio.h>
#include <string.h>

int main() {

    char str[5][20] = {
        "banana",
        "apple",
        "orange",
        "grape",
        "kiwi"
    };

    char temp[20];

    for (int i = 0; i < 4; i++) {

        for (int j = i + 1; j < 5; j++) {

            if (strcmp(str[i], str[j]) > 0) {

                strcpy(temp, str[i]);
                strcpy(str[i], str[j]);
                strcpy(str[j], temp);
            }
        }
    }

    for (int i = 0; i < 5; i++) {
        printf("%s\n", str[i]);
    }

    return 0;
}
```

# 28. diff

## 기능

두 파일의 내용을 비교합니다.

```c id="h0r74w"
#include <stdio.h>

int main() {

    FILE *f1 = fopen("file1.txt", "r");
    FILE *f2 = fopen("file2.txt", "r");

    int ch1, ch2;
    int line = 1;

    if (!f1 || !f2) {
        perror("diff 오류");
        return 1;
    }

    while ((ch1 = fgetc(f1)) != EOF &&
           (ch2 = fgetc(f2)) != EOF) {

        if (ch1 != ch2) {
            printf("차이 발견 : %d번째 줄\n", line);
            break;
        }

        if (ch1 == '\n')
            line++;
    }

    fclose(f1);
    fclose(f2);

    return 0;
}
```

# 29. uname

## 기능

운영체제 및 커널 정보를 출력합니다.

```c id="d8n3y5"
#include <stdio.h>
#include <sys/utsname.h>

int main() {

    struct utsname info;

    if (uname(&info) == 0) {

        printf("시스템 : %s\n", info.sysname);
        printf("노드명 : %s\n", info.nodename);
        printf("커널 버전 : %s\n", info.release);

    } else {
        perror("uname 오류");
    }

    return 0;
}
```

# 30. id

## 기능

현재 사용자의 UID와 GID 정보를 출력합니다.

```c id="4o8z3x"
#include <stdio.h>
#include <unistd.h>

int main() {

    printf("UID : %d\n", getuid());
    printf("GID : %d\n", getgid());

    return 0;
}
```
