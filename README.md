# Hash-encrypt-network
Idea of set up a hash encrypt network（HEN） to guarantee the security of cryptocurrency accounts.
目前有很多人因为安全问题在区块链上丢失了他们的财产，人们的虚拟资产正在巨大的风险阴影之中。
当下的区块链网络机制都不能完全规避密钥被暴露的风险。
真正的密钥需要完全离线。
我们可以在头脑中想一个密码，记为A，这个人我们假设为甲。
我们利用哈希函数H（），记B=H(A)。
我们创造一个这样的网络，将地址和密码根据哈希函数构建起对应关系。
比如上面这个例子我们就可以在这个网络中开一个地址为B的账户。
这个操作会显示在区块浏览器中，比如叫HenScan。
这个账户的密码就是A。
根据哈希函数不可逆的性质，他人可以看到有人创建了一个地址为B的节点，但是不能控制他，因为只有甲知道H^-1（B）的值。
上面是很平常的设定。
那么接下来，如果甲想将B账户（假设账户总金额为b）里的部分钱（假设金额为a）转账给乙，已知乙的地址是C。
然后，甲登录网络，对地址为的B账户签字一个密码输入合约，接下来网络会对地址为B的节点开始计时，甲需要在一段时间内（假设为t0）将A（也就是B对应的密码）输入并提交。
网络验证H（A）=B正确，甲获得B节点的控制权。假若在t0过程中网络收到了两份密码均为A或者没有接收到正确的密码，那么验证不通过。
下面说明无论何种情况A都将获得地址为B节点的控制权。
  1.如果甲签字密码输入合约之后没有输入密码，那么没有人可能知道密码（**A完全可以是甲自己在头脑中随意想出来的，只要甲不说，记住A或者找一个角落把密钥A写在纸上，A不触碰网络，没有人知道A是多少，选取合适的哈希函数，B也可通过A笔算得到，不需要任何现代装置**），那么t0后没有人可以输入正确的密码，验证不通过。账户验证通过后，在被控制时期内，任何人不能再提交密码来获取地址为B节点账户的控制权。
  2.如果甲签字密码输入合约之后开始输入了密码，如果甲在t0时间内完成了密码输入并提交，那么即使有人可以通过各种手段通过网络看到甲在输入的密钥，并且在过程中也尝试提交密码。那么只有两种可能：一是此人在t0内按照甲输入的密码输入密码并提交，那么网络会收到两份A密码，验证不通过；二是此人在t0后提交密码，此人验证无法通过，而甲则成功获得B节点的控制权。
  3.如果甲输入密码超时，则甲的资金有被盗的风险：有人可透过网络看到甲输入的内容，甲输入的内容若没有完全，则此人有利用部分信息爆破出A的可能；若甲完成输入但是没有提交，那么此人可直接将A复制粘贴密码并提交，导致在t0内此人获得了账户B的控制权。
所以甲的账户在此过程中被盗的可能仅限于第三点，故要合理设置t0的大小，既要留给甲充分的时间完成密码输入提交，又要尽可能减少网络的负担。需要后续落地实验来找到一个平衡。
在避免了3情况发生之后，甲100%得到节点B的控制权。
**接下来是最关键的部分**
那么这样看来“密钥”A也触网了，也泄露出去了？
甲可以再随便想一个密码A'，记H（A'）=B'。
甲获得地址为B节点账户控制权之后，可以在这个网络中签订账户迁移合约，把账户迁移到地址为B'的节点。根据上面的规则，这个过程中B节点除了甲没有其他人可以再登录进来。
**A密钥在这个操作后就失效了，因为账户迁移之后密码就从A变成了A'**
甲接下来可以利用A'来控制B'，于是此时甲的账户仍然是安全的，而节点B没有任何财产了（或极少量），即使有人知道甲之前输入的密钥A，也没办法在后面操纵甲的账户。
这时甲可以将B'中总金额为a的代币发送给乙的账户C，具体签订一个发送合约即可。
那么这之后甲要是担心自己的密钥A'泄露，那么只需再重复上面的步骤，再编一个密码进行账户迁移，A'就又失效了。
所以甲的资产可以达到完全安全的程度，不用担心被盗。
那么由此可见，这个新链（HEN）与传统链最大的差异在于有时钟系统，避免了临时密钥触网带来的风险；另外HEN链上在登录状态的账户具有排他性，这也是目前传统链所缺少的机制。
总体来看，Hdn能够对用户账户资金安全有更加完善的保障。节点用户的密钥是一次性的，只要操作过程合规，资产就没有风险。
而且值得注意的是，对于不追求过高的安全性的用户，也可以将HEN完全当做传统网络使用，我们结合上面的讨论，可以采用在传统网络的基础上添加一些额外安全设置可供客户调节和选择的方法。
HEN与传统链最大的区别在于为了使临时密钥成为可能，对密码机制进行了一定的更改（直接采用哈希映射来验证密码）。
而对追求高安全度的用户，这些新的东西正是为他们准备的，缺点可能就在于在频繁账户转移过程会增多资金的损耗。
注：以上仅是一种关于构建一个可以真正实现用脑钱包或者纸钱包参与区块链活动，而没有永久密钥触网的区块链网络模型的一个初步的想法。本人也初步接触区块链世界，还有很多很多问题值得关注和解决，希望可以抛转引玉。
