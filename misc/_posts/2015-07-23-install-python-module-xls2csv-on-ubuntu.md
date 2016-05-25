Python 命令行 Excel 转 csv 工具 xls2csv 安装记录

## 安装 cpanm
$ sudo apt-get install cpanminus

## 安装 gcc
$ sudo apt-get install gcc-4.6
$ which gcc-4.6
/usr/bin/gcc-4.6
$ sudo ln -sf /usr/bin/gcc-4.6 /usr/bin/cc

## 安装依赖模块
$ sudo cpanm --install Locale::Recode
$ sudo cpanm --install Unicode::Map
$ sudo cpanm --install Spreadsheet::ParseExcel
$ sudo cpanm --install Text::CSV_XS

## 安装 xls2csv
$ wget http://search.cpan.org/CPAN/authors/id/K/KE/KEN/xls2csv-1.07.tar.gz
$ tar -zxvf xls2csv-1.07.tar.gz && cd xls2csv-1.07
$ perl Makefile.PL
$ make
$ make test
$ make install
$ sudo make install
$ which xls2csv # 检查是否安装成功
/usr/local/bin/xls2csv


参考:
http://search.cpan.org/~ken/xls2csv-1.07/script/xls2csv



