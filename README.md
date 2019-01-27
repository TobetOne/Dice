# Tobet Dice Verify

验证工具网页请访问：https://tobetone.github.io/Dice/

在使用此网页工具之前，请仔细阅读以下说明。你可以根据如下说明，自行开发程序验证。
## 开奖结果计算
  1. 根据游戏数据生成随机种子,并签名：  
    seed  = GameId+Player+BetAmount+UserSeedSrc+BetTime;  
    seed_sign  = sign(seed);  
  2. 对随机种子hash（SH256）运算，结果转换为16进制  
    hash_hex = hex(sha256(seed_sign));   
  3. 根据 hash_hex 截取前6位转换数字之后 除以100取余再加1得到结果 
  
```javascript
    hash = SHA256 (seed_sign);
    hashSixInt = Math.abs (hexToInt(hash.substring(0,6)));
    result = hashSixInt % 100 + 1
```
## 随机因子说明
   playerSeed = GameId+Player+BetAmount+UserSeedSrc+BetTime;  
*  GameId:游戏ID;
*  Player:参与游戏的玩家账号;
*  BetAmount：投注金额;
*  UserSeedSrc:用户随机因子;
*  BetTime:玩家的投注时间时间戳;
## 签名验证
   验证签名使用Tobet的公钥（EOS6CG8VwJ8G1iFn6x781PMojmfD7i4kqqzsgd1AjWwAaEz35QGhn），对seed_sign进行ecc签名验证，验证结果通过即可证明随机种子未被篡改。
