# PHP7 is not compatible with Windows Server 2008 SP1

## PHP和Windows兼容性摘要
* *PHP 5.5+* 最低需要 Windows Server 2008 或者  Vista 的支持，32位或64位都一样
* 因此 *Windows XP* 最高支持 *PHP 5.4.x*
* PHP requires the Visual C runtime(CRT)
* PHP 5.5 and 5.6 require VC CRT 11 (Visual Studio 2012)
* PHP 7.0+ requires VC CRT 14 (Visual Studio 2015)
* VC14 CRT has many more DLLs(most in files with names starting with api-*)

## PHP7不能运行在 *Windows Server 2008 SP1* 上

针对最后一条，*Windows Server 2008 SP1* 一直报错： *api-ms-win-crt-runtime-l1-1-0.dll* 不存在，就是 VC14 的问题， *SP1* 即使反复安装 *VC14* 也无济于事，原因是 *KB2999226* 未安装，参考 [关于api-ms-win-crt-runtimel1-1-0.dll缺失的解决方案](http://blog.csdn.net/huqiao1206/article/details/50768481#%E5%AE%89%E8%A3%85vc-reditexe%E7%A8%8B%E5%BA%8F%E8%A7%A3%E5%86%B3) 。

郁闷的是：* Server 2008 SP1* 总是安装不上 *KB2999226* ，解决方案只能是升级到 *Server 2008 SP2* （上述文章只提到 *Win7* 必须升级到 *SP1* ， *Server 2008* 同理），这点网上基本没有资料提到，升级的包是 *KB948465* ，注意下载正确的位数版本。

## References
* [PHP Manual Installation and Configuration Installation on Windows systems](http://php.net/manual/en/install.windows.requirements.php)
