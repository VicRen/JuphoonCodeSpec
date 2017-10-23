#第一关：提交格式
##普通
* 必选内容不能缺失：标题、Bug ID、修改内容、测试用例
* 提交格式要正确，该格式严格检查，注意标题格式的4个部分和空格
* 测试用例要完整，至少包括测试步骤和期望结果，一条一个测试用例，一条可以写多行
* 没有change id	

```
正确格式示例：
bug: Android #6207 更新个人profile头像时，程序没有对图片进行压缩或者裁剪的操作，因此会提示上传失败

修改内容：
1.string文件中添加相应的提示内容
2.增加对上传头像图片格式的判断
3.增加对普通图片裁剪压缩及头像存储的方法

测试用例：
1.手机号码登录状态下，进入个人账户进行头像设置，仅支持支持格式的图片图片上传，若格式不符则进行提示然后重新设置。
2.手机号码登录状态下，进入个人账户进行头像设置，仅支持规定大小的普通图片，否则则进行裁剪压缩上传。
3.手机号码登录状态下，进入个人账户进行头像设置，仅支持规定大小的gif图片，否则则提示重新选择。

Smoke范围:
1.个人账户设置界面上传及更换头像

Change-Id: I1f07f74ceb0181ac3e8c0746b505358622b7ead5
```

##恶梦
* 测试用例要正确，要精确理解问题

##地狱
* 测试用例要充分，测试用例覆盖要全面，减少Regression。

#第二关：代码基本格式
##普通
* 代码中不应出现TAB和行尾空格，如果修改前就有，要将之前的修改好，如果改动太大则可以酌情考虑保留
* 自动生成的注释应删除或改成有意义的注释
```
就是这种：
// TODO Auto-generated method stub
```
* 代码缩进不正确，严重影响可读性。
```
反面教材：
    public void saveCompressPic(String picName,Bitmap mBitmap){
        File f = new File(RcsFileUtils.getSDKProfilesDir() + File.separator + MtcProf.Mtc_ProfGetCurUser() + File.separator  + picName + mPhotoType);
        Log.e("==========", f.getAbsolutePath());
        try {
         f.createNewFile();
        } catch (IOException e) {
         // TODO Auto-generated catch block
            e.printStackTrace();
        }
        FileOutputStream fOut = null;
        try {
         fOut = new FileOutputStream(f);
         int options = 100;
         mBitmap.compress(Bitmap.CompressFormat.PNG, options, fOut);
         while (mBitmap.getByteCount() / 1024>500) {
             fOut.flush();
             options -= 10;
             mBitmap.compress(Bitmap.CompressFormat.PNG, options, fOut);

         }
         fOut.flush();
         fOut.close();
        } catch (FileNotFoundException e) {
         e.printStackTrace();
        } catch (IOException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
      }
```
* 空格的使用规范
   * 语言关键字的括号后要加空格
   * 花括号代码块之前和之后如果有内容要加空格
```
if () {
} else {
}
switch () {
}
while () {
}
```
   * 表达式运算符前后要加空格
```
i = 0;
x == y;
x > 0 && z <= 100
p >= 0 ? x : y;
```
   * 数组类型不要加空格分隔类型名和中括号
```
反面教材：
int [] array;
规范写法：
int[] array;
```
* 命名不规范
* 单词拼写错误
* 不必要的缩写
* 与true和false比较

##恶梦
* 裸奔的常量，magic number
* 自动生成的无用代码应删除
* 英文不通顺

##地狱
* 命名不合适

#第三关：代码基本逻辑
##普通
* catch内没做任何有意义的处理
* 必要的地方没有try catch
* 不必要的反向逻辑
* 使用不必要的成员变量

#恶梦
* 没考虑空值(null,0,...)
* 使用了不明所以的代码

#第四关：代码整洁

##恶梦
* 无意义的修饰(final)应删除
* 过长的函数
* 逻辑过于混乱

##地狱
* 使用了复杂方法，其实有简单的途径

#第五关：代码优化

##恶梦
* 重复调用函数

##地狱
* 逻辑不完善

#第六关：Java
##普通
* 文件关闭有漏洞

#第七关：Android
##普通
* intent extra key常量放到其他activity中
