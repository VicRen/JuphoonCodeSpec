# 第一章 命名
## Item 1. 给常量命名
###释义

程序中的每一个数据都值得有一个名字，没有名字的常量通常叫做字面常量。使用字面常量会对代码造成如下影响：

1. 无法理解字面常量的含义，也许写代码的时候可以理解，但不要指望未来的自己或者别人能记住。这是对可读性造成的影响。
2. 多个地方使用同样的字面常量时候，无法直观地看出来逻辑上使用的是同一个值，因为可能不同目的的两个常量刚好是同样地值，从而造成混乱。这也是影响的可读性。
3. 当一个字面常量的值需要修改的时候，如果是字面常量，则需要手动一个一个地改，就算用查找替换也有改错的风险。如果定义了名字，就可以使用编辑器提供的查找替换或者更高级的重构功能来批量修改。这是对可维护性造成的影响。

###举例
一个显示所有联系人的列表中，在开始处需要显示3个固定的特殊的联系人，在计算列表大小的时候，需要考虑这几个特殊的联系人。
    
    public int getCount() {
        return mContactList.size() == 0 ? 3 : mContactList.size() + 4;
    }

当看到这个计算表达式的时候，不禁会问为什么又有3又有4呢？其实这个列表还有另外一个需求：当有联系人的时候，在最末尾显示一条显示数量的条目。用字面常量使这种复杂需求更深地隐藏在代码中。

当这些特殊的联系人有变化时，比如由3个变成了1个，通过搜索的方式查找3的数字这种方式就会把4漏掉，进而造成数量不对的隐患。

那么怎样定义常量来增强可读性，增加可维护性呢？上一段代码隐藏了两个逻辑的值，即特殊联系人的数量3，以及底部的显示数量的1。修改如下：

    public static final int TOP_ITEM_COUNT = 3;
    public static final int BOTTOM_ITEM_COUNT = 1;
    public int getCount() {
        if (mContactList.size() > 0) {
            return TOP_ITEM_COUNT + mContactList.size() + BOTTOM_ITEM_COUNT;
        } else {
            return TOP_ITEM_COUNT;
        }
    }

###例外情况

程序中经常会出现判断数量是否为0的场景，那么这个0需不需要定义成常量呢？那么经常判断的null也算是常量，需不需要定义成常量呢？

0可以表示很多种含义，如果是表示数量，那么它具有明确地意义，这时就不需要将其定义成常量，例如：

    int count = countMessages();
    if (count > 0) {
        return true;
    }

但如果0表示一组枚举值中的一个，那么0就失去了数量的意义。从本质上表示枚举的值可以是任何值，只要一组值之间能相互区分。这时就需要定义成常量，例如：

    public static final int MESSAGE_TYPE_TEXT = 0;
    public static final int MESSAGE_TYPE_IMAGE = 1;
    ...
    public static void showMessage(int type) {
        switch (type) {
            case MESSAGE_TYPE_TEXT:
                break;
            ...
        }
    }

对于null，它一直具有表示空值的特殊含义，而且几乎无法表示其他含义，也不应该被表示为其他的含义。因此它没有必要定义成常量，它本身就是一个含义清晰的符号。

###误用情况
命名一个字面常量的目的是增强可读性和可维护性，但有时为了命名而去命名会偏离命名的初衷，例如：

    public static final int RETURN_VALUE_1 = 1;
    public static final int RETURN_VALUE_2 = 2;
    public static final int RETURN_VALUE_3 = 3;

这种名字等于没有起名字，对程序的理解没有任何帮助，不知道123含义的人还是不知道；而且当有新的123字面值出现的时候，可能会误用这几个常量来表示，使得不同的逻辑值使用同一个常量，进而导致逻辑混乱造成隐患。

---

## Item 2. 变量使用名词，函数使用动词
###释义
变量一般表示数据和状态，应该用名词或者名词短语命名；而函数一般表示操作和动作，应该用动词或者动宾短语来命名。函数用动词，它的参数用名词，这样组合起来，可以很清晰地阅读一个函数函数的定义。

###举例
在一些弱类型语言中，由于变量本身并没有类型，只有靠命名表达变量的含义，这时命名就尤其重要。如下面的例子所示：

    var checkNumber;
    ...
    if (checkNumber) {
        showMyNumber();
    }
读这段代码，可以大概了解它的作用是检查是否存在号码并显示，但checkNumber这个变量可以有两种理解，一种是检查是否存在号码的布尔值，另一种是存放号码的字符串。产生这两种理解的原因是check前缀，如果只定义成number，那么它的意图肯定是存放号码字符串；如果定义成checkNumberResult就只表达检查号码结果。

###例外情况
一些函数是事件处理函数，是被动等待事件发生才调用的，因此事件函数的命名不能用动宾结构的短语。而是使用一些被动短语，如下：

    public void onGetTokenComplete(JSONObject jsonObj);
    public void onSearchPublicAccountsFail(int sessionId, int status);

布尔类型的变量一般会用is、has、need等be动词开头，表达的是一种状态：

    private boolean isAfternoon;
    private boolean hasRecipients;
    private boolean needRefresh;

## 不要缩写
## 不要在名字中加类型
## 警惕那些起名困难的变量和函数
