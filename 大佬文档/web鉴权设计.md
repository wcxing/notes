# 一、相关概念

## 1. web应用和web鉴权

鉴权的定义是，对用户访问系统进行权限控制。

web鉴权是针对web应用进行用户访问权限控制，那么什么是web应用呢？

从用户角度，使用一个web应用的过程就是一个对数据访问和操作的过程

从web应用的提供者角度，就是一个存储数据，提供数据，展示数据和提供数据操作方式的载体。

所以广义的web鉴权是指web应用使用者在使用过程中，web server对其进行访问权限控制。

而狭义的web鉴权是指web server在具体的接口中，判断接口参数中的用户标识是否有访问接口参数中指定想要访问的数据的权限。例如，一个接口参数为user_id=10010&book_id=10086，即用户10010想要10086这本书的信息，服务器需要判断10010用户是否有权限访问10086 book，这个过程可以称为“鉴权”。

## 2. web server 的职能

web server负责给用户提供数据服务。它需要考虑的事情有几个方面。

- 实现正常的功能，即提供数据的存储、访问和操作的能力
- 安全，机密、完整、可用
- 性能，并发访问
- 稳定性，其实也是可用性范畴

我们现在主要讨论“安全”中的鉴权相关内容。

## 3. 接口

接口是服务器提供服务的载体。接口接收**参数**，经过处理，返回**响应**。

鉴权就是在处理时候进行一些判断，确保响应中不包含用户无权限访问的数据。

## 4. 数据索引

数据索引，即"ID"，是用来在接口请求中标识想要访问的数据的参数。任何请求都需要包含数据索引，否则服务器不知道用户想要访问什么数据。

例如想要访问指定的书的数据的接口：/api/book/?user_number=10010&book_id=10086；想要访问指定的手机的数据的接口：/api/phone?user_number=95588&phone_id=12306，等等。

所有的数据索引都是服务器返回的。用户拿到数据索引后才能使用这个数据索引进行请求，访问关心的数据。比如对于博客应用的场景，用户登录后，服务器给用户颁发票据，用户拿这个票据访问"api/blog_list"，服务器给用户返回博客列表，列表中每项包含对应博客的id："blog_id"和信息，用户浏览博客列表信息后，还可以通过"blog_id"访问每个博客的详情。整个过程可以描述为 账号id + 密码 -> ticket -> blog id -> 博客内容。可以看出了ticket、blog id既是服务器返回的响应，也是下一个接口的请求参数。

通过上面的讨论可以知道，所有的数据索引都是服务器返回的，而最初的数据索引就是用户登录后服务器返回的ticket。

## 5. 鉴权和合法性校验

#### 1. 什么是鉴权

为了让用户访问其有权限访问的数据，而不能访问其无权限访问的数据，服务器需要进行鉴权。

用户都有哪些情况会访问无权访问的数据呢？主要有3种情况

1. 用户猜到其他数据索引，以此数据索引来请求自己无权限的数据
2. 防止数据索引和参数被盗
3. 防止用户分享数据索引和参数给其他人

服务器想要进行鉴权，必须要知道用户的标识。因此用户使用一个需要鉴权的web应用时候，第一步都是需要先登录，登录之后服务器会建立一个“会话”，服务器会给登录成功的用户发放一个有失效时间的“票据”，这个“票据”和该用户的用户标识绑定，用户使用“票据”可以和服务器正常通信，用户每个需要鉴权的接口都需要带上这个“票据”，服务器接收到请求中带有这个票据，服务器就知道自己在和哪个用户进行“会话”。当票据失效，“会话”也结束，需要重新登录。

只有通过用户“登录”、服务器发放“票据”、建立“会话”，服务器才能在无状态的http通信中知道发送请求的用户到底是谁。

通过“会话”机制，服务器可以知道发送请求的用户的身份，这样服务器就可以给用户提供其有权限访问的数据，而拒绝用户无权访问的数据的请求了。即服务器接收到一个请求时候，判断“票据”对应的用户是否有请求参数中数据索引对应的数据的访问权限，如果有权限就返回数据，如果没有权限就拒绝访问。这个合法性校验的过程我们称之为“狭义的鉴权”。为什么是“狭义”呢？因为有些场景这种合法性判断不适用，需要其他方式确保用户只访问自己有权限访问的数据。

web应用中，可能有这样的场景：维护用户的信息的服务器和实际提供web应用服务的服务器不是同一个服务器。比如，第三方服务A提供了IM应用服务，但是它不维护实际的用户数据，业务服务器B使用了A提供的服务，并且B维护自己的用户体系数据。那么A如何校验合法性呢？答案是应该由A和B约定一个规则"rule_foo"（md5的随机字符串或者des的秘钥），用户需要先登录B，然后用户访问IM服务时候，B会先进行鉴权，然后提供给用户一个经过约定规则“rule_foo”处理后的参数，用户使用这个参数访问IM服务，服务器A拿到参数之后，需要校验是否符合约定的规则。如果符合规则，说明是从服务器B发放出来的请求参数，而不是用户自定义的参数（因为规则只有A和B知道），认为用户有权限访问数据。服务器A的这种合法性校验也可以认为是校验用户是否有权限，即“鉴权”。

当然，服务器A也可以要求服务器B给用户提供请求参数时候包含“票据”，服务器A拿到“票据”再向服务器B发起请求进行“鉴权”，这种方式也可以实现用户访问权限判断。**为了避免中间人攻击伪造服务器B的返回的鉴权结果，这种请求应该使用https**。

所以广义的鉴权包括两种情况，一种是直接验证用户身份，看用户是否有权限访问想访问的数据。另一种是根据用户不可能知道的某个“规则”来进行校验，这样就可以避免用户通过猜到数据索引请求无权访问的数据。

#### 2. 接口请求合法性判断方法

**从校验的算法角度**：当服务器接收到一个请求时候（假设这个请求的参数一定是服务器发出去的，当然，在需要进行请求的合法性判断的场景中，除了登录之外，其他情况都符合这个前提），为了防止这个参数被用户篡改过，服务器需要保证它可以根据参数判断是否一定是自己发出去的。服务器如何能够保证这一点呢？

1. 这个参数有一定随机性，或者是经过加密处理，所以一定不会被用户猜到。那么服务器拿到一个参数时候如何知道是否是自己发出去的呢？答案是，自己记录一下自己发出去的所有的参数，拿到请求的参数时候，查一下这个参数是否在自己内存中，这样就可以判断参数是否是自己发出去的。sessionid就是这种情况。我们称这种方式为**校验记录**
2. 这个参数有一定规律，而且规律只有服务器自己知道，这样用户一定无法篡改。比如签名、参数加密都是这种情况。服务器校验规则，规则通过则认为鉴权通过。

对于第一种方式，有一些局限性，只能适用于发出数据索引和校验请求参数的服务器是同一个服务器的情况。第二种方式适用于发出数据索引和校验请求参数的服务器是同一台或者不是同一台的情况，只要不同服务器都知道同一个规则即可。我们称这种方式为**校验规则**

第一种方式因为信息都存在服务器，所以有更强的可控性，可以动态地修改，比如服务器在某一时刻可以撤销当前所有的ticket权限，只要在内存中销毁所有正在维护的ticket信息即可。又比如oahth2协议中，用户某一时刻希望对他颁发出去的token令牌（还在有效期内）撤销授权，那么对于第一种方式只要修改服务器对这个token的记录就行了。而第二种方式因为是校验规则，那么参数一旦确定就不容易动态修改了。

**从校验的方式角度**：从服务器发出的参数中是否包含用户标识和数据索引（还是只包含用户标识而不包含数据索引）？：

1. 如果经过算法处理的参数中  包含用户标识而不包含数据索引，那么服务器接收到请求后还需要校验该用户是否有权限访问该数据索引；（防止用户篡改自己的标识）我们称这种方式为**只处理用户标识，不绑定参数**
2. 如果经过算法处理的参数中  包含用户标识和数据索引，那么服务器接收到请求后不需要进行权限校验。（防止用户篡改参数）我们称这种方式为**绑定用户标识和参数，一起处理**

对于第一种方式，适用于可以通过用户信息进行用户权限校验的情况，第二种方式适用于没有维护用户信息所以不方便进行用户权限校验的情况。（当然，第二种方式也可以用于可以通过用户信息进行用户权限校验的情况，只要每次服务器返回的数据索引都按照规则进行处理、每次接收到请求后再按照规则进行校验即可，不过这样处理比较麻烦。第一种方式也可以用于没有维护用户信息所以不方便进行用户权限校验的情况，只要维护用户数据的用户颁发ticket，然后应用服务器使用ticket向用户信息服务器进行请求鉴权即可，当用户信息服务器希望对用户登录有更精准控制（比如互踢）时候，需要使用这种方式）

综上，具体使用哪种鉴权需要根据具体情况

1. 鉴权服务器是否有维护用户信息？

	这时候，应该“只处理用户标识，不绑定参数”，相对“绑定用户标识和参数，一起处理”的方式更简洁。
2. 鉴权是否想更精准地进行控制？（比如互踢和token等）

	这时候应该采用"校验记录"的方式，因为需要通过记录来进行精准控制

#### 3. 保证用户按照权限访问数据的方法

1. 鉴权、票据（类似于车票、盖章等）【用来防篡改】
2. token、签名（签名即一个除了数据索引之外的参数，用来承载一个只有服务器知道的规则、用户猜不到的规则）【用来防篡改】
3. 互踢【这是被分享出去的保险方案】
4. 回溯【(比如返回数据索引时候把用户标识和数据索引绑定并加签名防篡改或者DES加密)、这是被分享出去的保险方案】
5. 有效期【这是被分享出去的保险方案】

前两个手段解决用户猜数据访问无权限的数据，后3个手段解决分享数据索引的问题。

对于用户分享了数据索引（或者票据也一起分享）的情况，服务器想要验证请求的合法性就需要区分使用用户和分享到的用户。这是通过设备来近似区分的，根据设备可以做用户互踢的逻辑。

是否需要做互踢逻辑全看服务器有没有限制资源分享的权限，比如csdn.blog，这种用户完全没有分享自己账号的动机，所以没有必要做互踢限制。而对于boss直聘、腾讯视频这种，服务器不希望多个用户共享同一个账号（因为服务对用户vip账号提供有偿服务，用户就有动机分享自己的账号），因此可以做互踢的逻辑。

## 6. ticket、token和sign

在接口请求中，数据索引这个参数是必须的，因为只有知道数据索引服务器才能知道用户想要访问的数据是什么。另外还需要一些参数进行鉴权。在用不同方式进行鉴权的过程中，有这样一些参数：ticket、token和sign。下面介绍下这些参数。

1. ticket

	ticket票据，是用来标识一个用户合法身份的，是服务器先验证用户的权限（登录）之后颁发给用户的一个值。特点是1. 和指定用户相关  2. 有失效时间
	
	因为它是用户登录（即验证过账户id和密码）后服务器颁发的，因此服务器拿到这个ticket后会认为是可信的。

2. sign

	是对参数进行一系列操作然后md5后的结果，它用来防止用户篡改数据索引和请求参数

3. token

	token有两种含义：加密字符和令牌
	
	- 加密字符
		加密字符的作用和sign类似，也是提供一种只有服务器知道的规则，防止用户篡改数据索引，和sign不同的一点是，它可以对明文加密，提高请求参数的安全性。当然，**加密字符token**可以和sign一起使用。

	- 令牌
		令牌可以认为是一个临时票据，它可以不包含用户身份，用来给用户提供临时的数据访问权限，令牌有几个特点：
		1. 有效期短
		2. 可以被撤销
		3. 有权限范围


## 7. 重定向

重定向给我们带来哪些能力？用户不需要主动操作的情况下，服务器之间传递参数。适用于应用服务器依赖用户授权，而鉴权是在另一个服务器上，授权需要用户操作的场景中，用于简化用户操作完成正常业务逻辑。

# 二、一些原则

## 1. 最小信息权限

在保证正常业务逻辑实现的前提下，用户应该只能访问尽量少的数据。防止泄露非预期的信息。
	
## 2. 数据索引的散列
	
数据索引应该散列，而不应该自增或者有容易看出来的规律，防止用户猜到数据索引而访问自己无权限的数据，或者通过枚举的方式查到数据索引的范围（比如一个接口需要用到user_number参数，接口收到user_number判断如果没有的话，返回“无此用户”，而user_number是自增的，那么攻击者可以通过枚举user_number来测试出共有多少用户）。

## 3. https

尽量只提供https访问，防止中间人攻击

## 4. 签名、token一定是在鉴权之后颁发的

这是很基本的原则，是服务器能够信任sign、token的最基本的前提

## 5. 鉴权

为了保证最小信息原则，相关接口都应该进行鉴权，否则就是不安全的

## 6. 有效期

为了防止用户分享自己的请求参数给其他人，让其他用户也可以访问自己的数据，服务器返回的每个需要鉴权的（包含数据索引的）接口参数都应该是有有效期的。

# 三、合法性校验接口设计思路

#### 1. 系统设计的思路

1. 有哪些角色：资源服务器、应用服务器、鉴权服务器、用户等等
2. 每个角色应该负责做什么，生产和验证票据、serve应用、等等

#### 2. 鉴权系统设计思路：

1. 整个系统包含几个服务器，如果涉及多个服务器，每个服务器的职责是什么
2. 服务器应该暴露什么样的接口（接口给用户还是提供给服务器？），数据索引和返回结果是什么
3. 合法性校验是针对哪个具体的数据索引进行校验？
4. 用什么方式进行合法性校验？
	1. 是否是数据索引的生成和合法性校验不是同一个服务器？（是同一个服务器的话，通过鉴权进行合法性判断即可。如果不是，那可以通过进行校验的服务器拿到索引后到数据索引生成方去校验、或者两个服务器约定一个规则）
	2. 前端是否需要展示数据索引？（不需要的话，最好将索引加密处理）
	3. 是否需要进行动态控制？（需要的话，就得使用信息维护在服务器内存中）
5. 是否需要防止分享？还是不禁止分享？还是有可能有分享的需求？（防止分享的话，可以采用互踢、回溯和有效期等策略，不禁止分享则无需处理。如果有可能有分享需求但是需要控制权限，那么分享链接应该由服务器提供，获取分享链接前需要进行用户鉴权并获取用户标识，并且服务器将分享链接和用户标识绑定并做签名防止篡改）
6. 是否需要互踢？
7. 整体流程

