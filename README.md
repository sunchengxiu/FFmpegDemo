# ffmpeg 操作目录 实现 ls 显示目录的功能。

##### 打开目录

` // 打开目录 ret = avio_open_dir(&ctx, "./", NULL);`

##### 读取目录

`ret = avio_read_dir(ctx, &entry);`


##### 释放资源

`avio_free_directory_entry(&entry);`

`avio_close_dir(&ctx);`



# 代码，

```
#include<stdio.h>
#include<libavformat/avformat.h>
#include <libavutil/log.h>
int main(int argc , char * argv[]){
 int ret;
    av_log_set_level(AV_LOG_INFO);
    // 上下文
    AVIODirContext *ctx = NULL;
    AVIODirEntry *entry = NULL;
    // 打开目录
    ret = avio_open_dir(&ctx, "./", NULL);
    if (ret < 0) {
        av_log(NULL, AV_LOG_ERROR, "cant open dir");
        return -1;
    } else {
//        av_log(NULL, AV_LOG_INFO, "open success %s",av_err2str(ret));
    }

    while (1) {
    // 读取目录
        ret = avio_read_dir(ctx, &entry);
        if (ret < 0 ) {
            av_log(NULL, AV_LOG_ERROR, "cant read dir : %s",av_err2str(ret));
            // 防止内存泄漏
            goto __fail;
        } else {
//            av_log(NULL, AV_LOG_INFO, "read dir success");
        }
        // 末尾
        if (!entry) {
            break;
        }
        av_log(NULL, AV_LOG_INFO, "%8"PRId64" %s \n",entry->size , entry->name);
        // 释放资源
        avio_free_directory_entry(&entry);
    }
    // 释放资源
__fail:
    avio_close_dir(&ctx);
    return 0;
}



```
 
 