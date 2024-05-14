实现方式：通过清单直接实现com组件
来源：https://github.com/ikas-mc/ContextMenuForWindows11

exe实现方式：第一种变成msix包通过清单直接实现com组件，第二种配置一个msix包实现com组件并但是需要与exe 清单文件绑定
	主要是ContextMenuCustomHost这个项目,实现一个独立的dll

	第一种方式是将wpf打包为msix, https://learn.microsoft.com/zh-cn/windows/msix/packaging-tool/tool-overview

	第二种可以参考https://github.com/notepad-plus-plus/nppShell
		外部包授权方式：外部包无法直接安装，需要exe关联
			https://learn.microsoft.com/zh-cn/windows/apps/desktop/modernize/grant-identity-to-nonpackaged-apps
			demo : https://github.com/microsoft/AppModelSamples/tree/master/Samples/SparsePackages


打包文件：
	需要打包的 .\Packaging 清单.xml 文件夹
	打包后的文件名：SoftwareMenuexeapp.msix

	makeappx pack /d .\Packaging /p .\SoftwareMenuexeapp.msix /nv

签名文件：
	指纹："指纹值"
	签名文件：SoftwareMenuexeapp.msix

        证书签名： SignTool.exe sign /fd SHA256 /tr http://timestamp.digicert.com /td sha256 /a /f .\menutest.pfx /p 123456 /d "Notepad++" /du ContextMenuCustomHost.dll
	指纹签名：signtool.exe sign /tr http://timestamp.digicert.com /td sha256 /fd sha256 /sha1 "指纹" SoftwareMenuexeapp.msix

安装msix文件：
	Add-AppxPackage -Path "C:\Program Files (x86)\PDF Technologies, Inc\PDFTech Editor\NppShell.msix"
卸载msix文件：
	Get-AppxPackage -Name "PackageFullName" | Remove-AppxPackage

获取安装日志：
	Get-Appxlog

签名方案以及注意事项：

	https://github.com/ikas-mc/ContextMenuForWindows11/wiki/Create-Custom-Package
	https://learn.microsoft.com/en-us/windows/msix/package/sign-app-package-using-signtool
	https://learn.microsoft.com/en-us/windows/msix/package/unsigned-package
