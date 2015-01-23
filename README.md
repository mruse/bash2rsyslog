# bash2rsyslog
bash2rsyslog

### 
    # 下载最新版Bash
    wget http://ftp.gnu.org/gnu/bash/bash-4.3.tar.gz -P /usr/local/src/
    
    # 修改源码 bashhist.c
    vi bashhist.c +708  708行左右修改成下面内容
     syslog (SYSLOG_FACILITY|SYSLOG_LEVEL, "HISTORY: PID=%d PPID=%d SID=%d  User=%s CMD=%s", getpid(), getppid(), getsid(getpid()),  current_user.user_name, line);
  else
    {
      strncpy (trunc, line, SYSLOG_MAXLEN);
      trunc[SYSLOG_MAXLEN - 1] = '\0';
      syslog (SYSLOG_FACILITY|SYSLOG_LEVEL, "HISTORY (TRUNCATED): PID=%d  PPID=%d SID=%d User=%s CMD=%s", getpid(), getppid(), getsid(getpid()),  current_user.user_name, trunc);
    }
    # 修改源码 bashhist.c
    vi config-top.h  取消/*#define SYSLOG_HISTORY*/这行的注释
    
    # 解压、编译、安装
    tar xvf bash-4.3.tar.gz -C /usr/local/src/
    cd /usr/local/src/bash-4.3
    ./configure --prefix=/usr/local/
    make
    make install
    
    # 替换
    chattr -i /etc/passwd /etc/shadow
    sed -i 's#/bin/bash#/usr/local/bin/bash#g' /etc/passwd
    chattr +i /etc/passwd /etc/shadow
    cd -
    rm -rf bash-4.3.tar.gz /usr/local/src/bash-4.3/
