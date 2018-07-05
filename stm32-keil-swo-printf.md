# stm32l476在keil里开swo调试输出printf
## 硬件配置
nucleol476rg开发板自带的st-linkv2调试工具，已经将stm32l476的swo(PB3)管脚和调试工具连接好。
## keil设置
打开“Option for torget"配置，选择debug中的use:ST-Link Debugger,点击“Setting”设置界面，在trace栏中，先在“Trace Enable”前打对勾，在"Core Clock"设置单片机主频，将ITMStimulus Ports下将Enble设置为0x00000001，Privilege设置为0x00000000。

## 代码添加printf

1、先定义宏定义：
Add ITM Port register definitions to your source code. 
```c
#define ITM_Port8(n)    (*((volatile unsigned char *)(0xE0000000+4*n)))
#define ITM_Port16(n)   (*((volatile unsigned short*)(0xE0000000+4*n)))
#define ITM_Port32(n)   (*((volatile unsigned long *)(0xE0000000+4*n)))

#define DEMCR           (*((volatile unsigned long *)(0xE000EDFC)))
#define TRCENA          0x01000000
```
2、定义prinft
Add an fputc function to your source code that writes to the ITM Port 0 register. The fputc function enables printf to output messages.
```c
struct __FILE { int handle; /* Add whatever you need here */ };
FILE __stdout;
FILE __stdin;

int fputc(int ch, FILE *f) {
  if (DEMCR & TRCENA) {
    while (ITM_Port32(0) == 0);
    ITM_Port8(0) = ch;
  }
  return(ch);
}
```
3、添加printf打印信息
Add your debugging trace messages to your source code using printf.
```c
printf("AD value = 0x%04X\r\n", AD_value);
```
