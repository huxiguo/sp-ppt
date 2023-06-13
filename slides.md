---
theme: seriph
background: https://source.unsplash.com/collection/94734566/1920x1080
class: 'text-center'
css: unocss
transition: fade-out
---

<v-click>

# 手机门禁代码开发

</v-click>

<v-click>

### <logos-mailchimp-freddie/> 硬件代码开发

</v-click>

<v-click>

### <logos-android-icon/> Android APP开发
</v-click>

<v-click>

### <logos-chromatic-icon/> 卡片管理APP开发
</v-click>

---
css: unocss
layout: image-right
image: https://source.unsplash.com/collection/94734566/1920x1080
---

#  <logos-mailchimp-freddie/> 硬件代码开发

<v-clicks>

1. <bx-rfid/>  读卡器开发
2. <material-symbols-lock/> 电子锁开发
  
</v-clicks>

---
css: unocss
highlighter: shiki
---

# <bx-rfid/>  读卡器开发


<div class="flex flex-col">

入口函数
  <div>

```c {all|3}
int main(void)
{
	init(); // 初始化
	mode(); // 实现功能
}
```
  </div>

  <div>

<v-clicks>

init() 函数

```c {3|4|5|6|7|all}
void init(void)
{
	rcc_init(); // 时钟初始化
	tim2_init(); // 定时器初始化
	usart1_init(9600); // 串口初始化						
	sim_configuration(); // 2.4G模块初始化
	usart3_init(115200); // 串口初始化
}
```
</v-clicks>
  </div>
</div>

---
css: unocss
highlighter: shiki
---

# <bx-rfid/>  读卡器开发


<div class="flex flex-col">
  <div>

init() 函数

```c {all|3}
void init(void)
{
	rcc_init(); // 时钟初始化
	tim2_init(); // 定时器初始化
	usart1_init(9600); // 串口初始化						
	sim_configuration(); // 2.4G模块初始化
	usart3_init(115200); // 串口初始化
}
```
  </div>



  <div>
  <v-clicks>

rcc_init() 函数

```c {3|4|5|all}
void rcc_init(void)
{
  RCC_ClocksTypeDef RCC_ClockFreq; // 定义时钟结构体
	SystemInit(); // 系统初始化 stm32f10x库文件中的函数，直接调用即可
	RCC_GetClocksFreq(&RCC_ClockFreq); // 获取时钟频率
}
```
</v-clicks>
  </div>

</div>

---
css: unocss
highlighter: shiki
---

# <bx-rfid/>  读卡器开发


<div class="flex">
  <div class="mr-1">

init() 函数

```c {all|4|5|6}
void init(void)
{
	rcc_init(); // 时钟初始化
	tim2_init(); // 定时器初始化
	usart1_init(9600); // 串口初始化						
	sim_configuration(); // 2.4G模块初始化
	usart3_init(115200); // 串口初始化
}
```
  </div>



  <div>
  <v-clicks>
  
sim_configuration() 

```c {3,4|5,6|7,8,9|10,11,12|13,14|all}
void sim_configuration(void)
{
  // PB ,AFIO总线
  RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOA | RCC_APB2Periph_AFIO, ENABLE); //使能GPIOA时钟
  // 使能USART2时钟
  RCC_APB1PeriphClockCmd(RCC_APB1Periph_USART2, ENABLE); //使能USART2时钟
  // A2 做为USART2的Tx
  GPIO_InitStructure.GPIO_Pin = GPIO_Pin_2; //PA2
  GPIO_Init(GPIOA, &GPIO_InitStructure);
  // A3 做为USART2的Rx
  GPIO_InitStructure.GPIO_Pin = GPIO_Pin_3; //PA3
  GPIO_InitStructure.GPIO_Mode = GPIO_Mode_IN_FLOATING; //浮空输入
  OFF_SIM_RX_DATA; //关闭接收中断
  OFF_SIM_TX_DATA; //关闭发送中断
}
```
</v-clicks>
  </div>

</div>

---
css: unocss
highlighter: shiki
---

# <bx-rfid/>  读卡器开发


<div class="flex">
  <div class="mr-1">

init() 函数

```c {all|7}
void init(void)
{
	rcc_init(); // 时钟初始化
	tim2_init(); // 定时器初始化
	usart1_init(9600); // 串口初始化						
	sim_configuration(); // 2.4G模块初始化
	usart3_init(115200); // 串口初始化
}
```
  </div>



  <div>
  <v-clicks>
  
sim_configuration() 

```c {3,4,5|7,8|10,11|13,14|16,17,18|all}
void usart3_init(u32 baud)
{
  RCC_APB2PeriphClockCmd( RCC_APB2Periph_GPIOB | RCC_APB2Periph_AFIO , ENABLE); // 使能GPIOB时钟
  RCC_APB1PeriphClockCmd( RCC_APB1Periph_USART3 , ENABLE); // 使能USART3时钟
	NVIC_Init(&NVIC_InitStructure); // 初始化中断结构体

  GPIO_InitStructure.GPIO_Pin = GPIO_Pin_10; // PB10
  GPIO_Init(GPIOB, &GPIO_InitStructure); // 初始化GPIO

  GPIO_InitStructure.GPIO_Pin = GPIO_Pin_11; // PB11
  GPIO_InitStructure.GPIO_Mode = GPIO_Mode_IN_FLOATING; // 浮空输入

  GPIO_Init(GPIOB, &GPIO_InitStructure); // 初始化GPIO
  USART_Init(USART3, &USART_InitStructure); // 初始化串口

  USART_Cmd(USART3, ENABLE); // 使能串口
  USART_ITConfig(USART3,USART_IT_RXNE,ENABLE); // 使能接收中断
  USART_ClearFlag(USART3, USART_FLAG_TC); // 清除发送完成标志
}
```
</v-clicks>
  </div>

</div>

---
css: unocss
highlighter: shiki
---

# <bx-rfid/>  读卡器开发


<div class="flex flex-col">

入口函数
  <div>

```c {all|4}
int main(void)
{
	init(); // 初始化
	mode(); // 实现功能
}
```
  </div>

  <div>

<v-clicks>

mode() 函数

```c {3|4|5|6|all}
void mode(void)
{
	OSInit(); // 初始化UCOS
	OSTaskCreate( StartTask,(void *) 0,(OS_STK *)&StartTaskStk[START_STK_SIZE - 1],(INT8U )START_TASK_PRIO);  // 创建开始任务
  OSStart(); // 启动UCOS，
	while(1); // 死循环，防止程序跑飞
}
```
</v-clicks>
  </div>
</div>

---
css: unocss
highlighter: shiki
---

# <bx-rfid/>  读卡器开发


<div class="flex flex-col">
  <div>

init() 函数

```c {3|4|5|6|4}
void mode(void)
{
	OSInit(); // 初始化UCOS
	OSTaskCreate( StartTask,(void *) 0,(OS_STK *)&StartTaskStk[START_STK_SIZE - 1],(INT8U )START_TASK_PRIO);  // 创建开始任务
  OSStart(); // 启动UCOS，
	while(1); // 死循环，防止程序跑飞
}
```
  </div>

  <div>
  <v-clicks>

rcc_init() 函数

```c {all|3|4|5|7,8,9}
void StartTask(void *pdata)
{
  OS_CPU_SysTickInit(); // 初始化系统滴答定时器
  #if (OS_TASK_STAT_EN > 0) // 如果使能了统计任务
    OSStatInit(); // 初始化统计任务
  #endif
 	  OSTaskCreate(led_task,(void *)0,(OS_STK*)&LED_TASK_STK[LED_STK_SIZE-1],LED_TASK_PRIO); // 创建LED任务
	  OSTaskCreate(beep_task,(void *)0,(OS_STK *)&BEEP_TASK_STK[BEEP_STK_SIZE - 1],(INT8U )BEEP_TASK_PRIO); // 创建蜂鸣器任务
	  OSTaskCreate(sim_task,(void *) 0,(OS_STK *)&SIMTaskStk[SIM_STK_SIZE - 1],(INT8U)SIM_TASK_PRIO); // 创建SIM任务
}
```
</v-clicks>
  </div>
</div>

---
css: unocss
highlighter: shiki
---

# <bx-rfid/>  读卡器开发

sim_task() 主要代码

```c
// 读卡
if (usart3_rev_buff[6] == IGW_TO_NODE_READ_CARD) {
  LED2(ON); // 红灯亮
    usart3_to_a9(MY_DeviceType, MY_SensorType, MY_DeviceAddrNoH, MY_DeviceAddrNoL, NODE_TO_IGW_DATA_ACK, rbuff, 0); // 发送ACK，告诉A9已经收到数据，可以发送下一条数据了
    read_card_timeout = 0; // 超时时间清零，重新计时
    while (sim_reday_press() != 0) { // 等待卡片靠近
      OSTimeDlyHMSM(0, 0, 0, 100); // 延时100ms，等待卡片靠近
      if (read_card_timeout > 500) break; // 如果超时时间大于500ms，跳出循环
    }
    if (sim_read_block(SimBlock_File, &rbuff[8]) == SIMRC_OK) // 读取卡片数据
      {
        usart3_to_a9(MY_DeviceType, MY_SensorType, MY_DeviceAddrNoH, MY_DeviceAddrNoL, NODE_TO_IGW_CARD_DATA, SIM_ICID_BAK_LAST, 8); // 发送卡片数据
        memset(SIM_ICID_BAK_LAST, 0, 8); // 清空卡片数据
        OSSemPost(BeepSem); // 发送蜂鸣器信号量
        OSSemPost(LEDIRQSem); // 发送LED信号量
      } else {
          usart3_to_a9(MY_DeviceType, MY_SensorType, MY_DeviceAddrNoH, MY_DeviceAddrNoL, NODE_TO_IGW_READ_CARD_FAIL, rbuff, 0); // 发送读卡失败
        }
        LED2(OFF); // 红灯灭
}
```

---
css: unocss
highlighter: shiki
---

# <bx-rfid/>  读卡器开发

读卡操作

```c
s8 sim_read_block(u8 block,u8* data) {
	sim_send_pasv(); // 发送pasv指令
	while(SIM_TX_DATA.SIM_NUM_STR != 0); // 等待发送完成
	SIM_RX_DATA.SIM_BUF[16] = 0; // 清空接收缓冲区
	ON_SIM_RX_DATA; // 开启接收中断
	OSSemPend(SIMIRQSem,SIM_TRIG_ONTIME,&err); // 等待接收中断
	if(OS_NO_ERR == err) { // 如果没有错误
		if(SIM_RX_DATA.SIM_CMD == SIMRBLCCMD) { // 如果接收到的是读卡指令
			if(SIM_RX_DATA.SIM_BUF[16] == 0x90 && SIM_RX_DATA.SIM_BUF[17] == 0x00 && SIM_RX_DATA.SIM_BUF[18] == 0x00) { // 如果接收到的是正确的数据
				memcpy(data,SIM_RX_DATA.SIM_BUF,16); // 将数据拷贝到data中
				return SIMRC_OK; // 返回读卡成功
			}else return SIMRC_READDATA_FAIL; // 返回读卡失败
		} else return SIMRC_CMD_FAIL ; // 返回指令错误
  } else return SIMRC_TIME_OUT; // 返回超时
}
```

---
css: unocss
highlighter: shiki
---

# <bx-rfid/>  读卡器开发

写卡操作

```c
s8 sim_write_block(u8 block,u8* data) {
	#define SIMWBLCCMD 0X20 // 写卡指令
	u8 err; // 定义错误变量
	memcpy(&SIM_TX_DATA.SIM_BUF[5],data,16); // 将数据拷贝到发送缓冲区
	SIM_TX_DATA.SIM_NUM_STR = SIM_TX_DATA.SIM_DATA_LEN + SIM_READER_MODEL_LEN; // 设置发送长度 
	ON_SIM_TX_DATA; // 开启发送中断
	sim_send_pasv(); // 发送pasv指令
	while(SIM_TX_DATA.SIM_NUM_STR != 0); // 等待发送完成
	SIM_RX_DATA.SIM_BUF[0] = 0; // 清空接收缓冲区
	ON_SIM_RX_DATA; // 开启接收中断
	OSSemPend(SIMIRQSem,SIM_TRIG_ONTIME,&err); // 等待接收中断
	if(OS_NO_ERR == err) { // 如果没有错误
		if(SIM_RX_DATA.SIM_CMD == SIMWBLCCMD) {	 // 如果接收到的是写卡指令
			if(SIM_RX_DATA.SIM_BUF[0] == 0x90 && SIM_RX_DATA.SIM_BUF[1] == 0x00 && SIM_RX_DATA.SIM_BUF[2] == 0x00) // 如果接收到的是正确的数据
				return SIMRC_OK; // 返回写卡成功
			else return SIMRC_WRITEDATA_FAIL; // 返回写卡失败
		}
		else return SIMRC_CMD_FAIL ; // 返回指令错误
	}
	else return SIMRC_TIME_OUT;  // 返回超时
}
```

---
css: unocss
highlighter: shiki
---

# <material-symbols-lock/> 电子锁代码开发

电子锁初始化

```c
void lock_init(void)
{
	GPIO_InitTypeDef GPIO_InitStructure;
	RCC_APB2PeriphClockCmd( RCC_APB2Periph_GPIOC, ENABLE);
	GPIO_InitStructure.GPIO_Pin = GPIO_Pin_9;	
	GPIO_InitStructure.GPIO_Mode = GPIO_Mode_Out_PP;       
	GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;
	GPIO_Init(GPIOC, &GPIO_InitStructure);
	GPIO_ResetBits(GPIOC,GPIO_Pin_9);
}
```

---
css: unocss
highlighter: shiki
---

# <material-symbols-lock/> 电子锁代码开发

lock 任务

```c
void lock_Task(void *pdata){
  while (1){
    // 如果接收完成
		if (flag_rev_finish == 1) {
      // 清除接收完成标志
			flag_rev_finish = 0; 
      // 如果接收到的数据是正确的
			if ((usart3_rev_buff[0] == 0xff) && (usart3_rev_buff[1] == 0x55) && (usart3_rev_buff[usart3_rev_buff_cnt - 2] == 0xfe) && (usart3_rev_buff[usart3_rev_buff_cnt - 1] == 0xaa)) 
      {
        if (usart3_rev_buff[2] == RF_Lock_Device && usart3_rev_buff[6] == IGW_TO_NODE_OPEN_LOCK){
          // 开锁
        }
				else if (usart3_rev_buff[2] == RF_Lock_Device && usart3_rev_buff[6] == IGW_TO_NODE_CLOSE_LOCK){
					// 关锁
				}
				else usart3_rev_buff_cnt = 0; // 清空缓冲区，重新接收
			}
			else memset(usart3_rev_buff, 0, usart3_rev_buff_cnt); // 清空缓冲区
			usart3_rev_buff_cnt = 0; // 清空缓冲区
		}
		OSTimeDlyHMSM(0, 0, 0, 50); // 延时50ms
	}
}
```

---
css: unocss
highlighter: shiki
---

# <material-symbols-lock/> 电子锁代码开发

开锁

```c
usart3_to_a9(RF_Lock_Device, NODE_TO_TGW_DATA_ACK, 0, 0); // 发送开锁指令
OSTimeDlyHMSM(0, 0, 0, 200); // 延时200ms
memset(usart3_rev_buff, 0, usart3_rev_buff_cnt); // 清空缓冲区
usart3_rev_buff_cnt = 0; // 清空缓冲区
GPIO_SetBits(GPIOC, GPIO_Pin_9); // 开锁
OSSemPost(BeepSem); // 发送蜂鸣器信号量
OSSemPost(LEDIRQSem); // 发送LED信号量
OSTimeDlyHMSM(0, 0, 5, 0); // 延时5s
GPIO_ResetBits(GPIOC, GPIO_Pin_9); // 关锁
```
---
css: unocss
highlighter: shiki
---

# <material-symbols-lock/> 电子锁代码开发

关锁

```c
usart3_to_a9(RF_Lock_Device, NODE_TO_TGW_DATA_ACK, 0, 0); // 发送关锁指令
OSTimeDlyHMSM(0, 0, 0, 200); // 延时200ms
memset(usart3_rev_buff, 0, usart3_rev_buff_cnt); // 清空缓冲区
usart3_rev_buff_cnt = 0;  // 清空缓冲区
GPIO_ResetBits(GPIOC, GPIO_Pin_9); // 关锁
OSSemPost(BeepSem); // 发送蜂鸣器信号量
OSSemPost(LEDIRQSem); // 发送LED信号量
```          

---
css: unocss
layout: image-right
image: https://source.unsplash.com/collection/94734566/1920x1080
---

#  <logos-android-icon/> Android APP开发

<v-clicks>

1. <material-symbols-credit-card-outline/> 门禁刷卡页面
2. <mdi-cards-outline/> 开卡页面
3. <mdi-feature-search-outline/> 查询页面
  
</v-clicks>

---
css: unocss
highlighter: shiki
---

# <logos-android-icon/> Android APP开发

门禁刷卡页面

```java
Handler handler = new Handler(){
		@Override
		public void handleMessage(Message msg){
			if(msg.what == 1){
				if(MyConfig.getMac().get(SensorType.RF_Lock_Device)!=null){
				  uart.send_ByteUart(SendOrder.zhaji(SensorType.IGW_TO_NODE_OPEN_LOCK,SensorType.RF_Lock_Device,MyConfig.getMac().get(SensorType.RF_Lock_Device)));//发送打开锁命令
				}
				animation.start(); //开锁动画
				Timer timer = new Timer(); 
				TimerTask task = new TimerTask() {
					@Override
					public void run() {
						animation.stop(); //停止动画
						if(MyConfig.getMac().get(SensorType.RF_Lock_Device)!=null)
							   uart.send_ByteUart(SendOrder.zhaji(SensorType.IGW_TO_NODE_CLOSE_LOCK,SensorType.RF_Lock_Device,MyConfig.getMac().get(SensorType.RF_Lock_Device))); //发送关闭锁命令
					}
				};
				timer.schedule(task, 5200); 
			}
		}
	};
```

---
css: unocss
highlighter: shiki
---

# <logos-android-icon/> Android APP开发

开卡页面

```java
public void onClick(View v) {
	if(et_id.getText().toString().isEmpty()){ //判断卡号是否为空
		Toast.makeText(getApplicationContext(), "请刷卡！", 0).show();
		return;
	}
	if(et_name.getText().toString().isEmpty()){ //判断用户名是否为空
		Toast.makeText(getApplicationContext(), "用户名不能为空！", 0).show();
		return;
	}
	if(!et_name.getText().toString().matches(regname)){ //判断用户名是否合法
		Toast.makeText(getApplicationContext(), "用户名只能为2-10位的汉字或英文！", 0).show();
		return;
	}
	if(userDao.findPersonById(et_id.getText().toString())!=null){
		userDao.delUserById(et_id.getText().toString()); //当该卡已开过户，则将之前的信息删除
	}
	User user=new User(et_id.getText().toString(), et_name.getText().toString(), System.currentTimeMillis(), "用户开卡"); //将用户信息存入数据库
	userDao.addUser(user); //将用户信息存入数据库
	Toast.makeText(getApplicationContext(), "开卡成功！", 0).show(); //提示开卡成功
	this.finish(); //关闭当前页面
}
```
---
css: unocss
highlighter: shiki
---

# <logos-android-icon/> Android APP开发

查询信息

```java
// 根据用户名查询用户信息
public List<User> getListByName(String name) {
		List<User> personlist = new ArrayList<User>(); //创建用户信息列表
		User user = null; //创建用户信息对象
		Cursor cursor = mydb.query("person",new String[] { "id,name,date,thing" }, "name=?", new String[] { name }, null, null,"date desc"); //查询数据库
		while (cursor.moveToNext()) {
			@SuppressLint("Range") String id = cursor.getString(cursor.getColumnIndex("id")); //获取卡号
			@SuppressLint("Range") long date = cursor.getLong(cursor.getColumnIndex("date")); //获取开卡时间
			@SuppressLint("Range") String thing = cursor.getString(cursor.getColumnIndex("thing")); //获取开卡时间
			user = new User(id, name, date, thing); //创建用户信息对象
			personlist.add(user); //将用户信息对象添加到用户信息列表
		}
		return personlist; //返回用户信息列表
	}
```
---
css: unocss
layout: image-right
image: https://source.unsplash.com/collection/94734566/1920x1080
---

#  <logos-chromatic-icon/> 卡片管理APP开发

<v-clicks>

1. <material-symbols-login/> 登录页面
2. <ic-round-person-search/> 查询功能
3. <material-symbols-delete-outline/> 删除功能
  
</v-clicks>

---
css: unocss
highlighter: shiki
---

# <logos-chromatic-icon/> 卡片管理APP开发

登录页面

```java
editTextUsername = findViewById(R.id.editTextUsername); // 获取用户名输入框
editTextPassword = findViewById(R.id.editTextPassword); // 获取密码输入框
buttonLogin = findViewById(R.id.buttonLogin); // 获取登录按钮
buttonLogin.setOnClickListener(new View.OnClickListener() { // 设置登录按钮点击事件
 @Override
public void onClick(View v) {
  String username = editTextUsername.getText().toString(); // 获取用户名
  String password = editTextPassword.getText().toString(); // 获取密码
  if (validateCredentials(username, password)) { // 验证用户名和密码
    Intent intent = new Intent(LoginActivity.this, MainActivity.class); // 跳转到主页面
    startActivity(intent); // 启动主页面
    finish();  // 关闭当前登录页面
  } else {
      Toast.makeText(LoginActivity.this, "密码错误", Toast.LENGTH_SHORT).show(); // 提示密码错误
    }
}
});
```
---
css: unocss
highlighter: shiki
---

# <logos-chromatic-icon/> 卡片管理APP开发

删除功能

```java
 private void deleteData(String name) {
  // 执行删除操作
  int rowsDeleted = dbHelper.deleteData("person", "name=?", new String[]{name}); // 删除指定用户名的用户信息
  if (rowsDeleted > 0) {
    Toast.makeText(this, "删除成功", Toast.LENGTH_SHORT).show(); // 提示删除成功
      queryData(); // 查询用户信息
  } else {
      Toast.makeText(this, "未找到匹配项", Toast.LENGTH_SHORT).show(); // 提示未找到匹配项
    }
}
```

---
css: unocss
highlighter: shiki
---

# <logos-chromatic-icon/> 卡片管理APP开发

查询功能


```java
private void queryData() { // 执行查询操作，并获取结果的Cursor对象
        Cursor cursor = dbHelper.queryData("person", new String[]{"id","name", "date","thing", "id AS _id"}, null, null, null, null, null);
        int columnIndexDate = cursor.getColumnIndex("date"); // 列索引
        String[] columns = {"id","name", "date", "thing", "_id"}; // 创建新的 MatrixCursor 对象，用于存储转换后的日期
        MatrixCursor newCursor = new MatrixCursor(columns);
        if (cursor.moveToFirst()) { // 遍历原始 Cursor 中的每一行
            do {
                long timestamp = cursor.getLong(columnIndexDate);// 获取原始的时间戳值
                String formattedDate = convertTimestampToDateString(timestamp); // 将时间戳转换为正常的日期格式
                // 从原始 Cursor 中读取其他列的值
                @SuppressLint("Range") String id = cursor.getString(cursor.getColumnIndex("id"));
                @SuppressLint("Range") String nameValue = cursor.getString(cursor.getColumnIndex("name"));
                @SuppressLint("Range") String thingValue = cursor.getString(cursor.getColumnIndex("thing"));
                @SuppressLint("Range") long idValue = cursor.getLong(cursor.getColumnIndex("_id"));
                newCursor.addRow(new Object[]{id,nameValue, formattedDate, thingValue, idValue}); // 添加转换后的日期和其他列的值到新的 Cursor 中
      } while (cursor.moveToNext());
  }
  String[] fromColumns = {"id","name", "date", "thing"};
  int[] toViews = {R.id.textId,R.id.textName, R.id.textDate, R.id.textThing};
  SimpleCursorAdapter adapter = new SimpleCursorAdapter(this, R.layout.list_item, newCursor, fromColumns, toViews, 0);// 创建一个适配器（Adapter），将查询结果绑定到ListView上
  listResults.setAdapter(adapter);// 设置适配器
}
```


---
layout: center
class: text-center
---

# Thank you!
