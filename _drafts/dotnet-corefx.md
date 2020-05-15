https://github.com/dotnet/corefx/blob/master/Documentation/building/advanced-inner-loop-testing.md

cd ~/repos/corefx/

./build.sh -release

rm /home/peter/Documents/ProcessNameTest/ProcessNameTest/runtime/* -rf

cp artifacts/bin/testhost/netcoreapp-Linux-Release-x64/shared/Microsoft.NETCore.App/9.9.9/* /home/peter/Documents/ProcessNameTest/ProcessNameTest/runtime/ -r

cd ~/Documents/ProcessNameTest/ProcessNameTest/runtime/

./corerun /home/peter/repos/corefx/Tools/csc.dll /noconfig /r:System.Runtime.dll /r:System.Runtime.Extensions.dll /r:System.Runtime.InteropServices.dll /r:System.Text.Encoding.Extensions.dll /r:System.Threading.dll /r:System.Linq.dll /r:System.dll /r:System.Reflection.dll /r:System.Diagnostics.Process.dll /r:System.Data.dll /r:System.Private.CoreLib.dll /r:System.Console.dll /r:System.ComponentModel.Primitives.dll /r:System.Collections.NonGeneric.dll  /out:Program.dll ../Program.cs

./corerun Program.dll
