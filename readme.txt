pad 2.0 version :
order excel data save path : orders(excel)
clock out data path : empolyeesData

commit
----------------------------------------------------------------------------------------
fix: 選擇打折後再添加減少物品金額暫時錯亂、文本放置空行問題、未遵循一個功能一個Class寫法

問題：
1. 原程式碼更新金額時邏輯未寫好，導致按下勾選後變數金額未更新。
2. 原程式碼使用插件事插入，當其中一個品項未放入時導致會空一行。
3. 未發生問題，但擴展性差。

原因：
1. 按下vip鍵後，金額變數沒有更新，導致頁面呈現暫時抓取到未更新資料。
2. 置入文檔儲存格式時需未購買商品保留文檔未置。
3. 直述性寫法。

調整項目：
1. 改寫變數取值邏輯。
2. 使用多個if將所有可狀況列舉出來再一次性更新文檔。
3. 內容全部重構。

----------------------------------------------------------------------------------------
feat:1. Sign.java 新增使者註冊系統

	可註冊多個使用者及密碼，並拒絕使用者名稱相同。

調整項目：
1.addUser() 負責寫入使用者資料。
2.read() 讀取使用者資料。
3.bool() 判斷使用者名稱有無重複，回傳布林值
4.check() 比對帳號密碼 回傳布林值


----------------------------------------------------------------------------------------

feat:2. OutputExcel.java 寫入匯出Excel檔案
	
	使用poi.jar作為套件。
	依照結帳時間、訂單順序(序號自動增加)作為檔名寫入匯出Excel檔案。

調整項目：
1.createExcel() 創建、寫入Excel。

----------------------------------------------------------------------------------------

feat:3. WorkTimer.java 上、下班打卡系統

	可記錄該員工上班時間、下班時間、總工作時間，並依員工編號及日期匯出檔案
	也能當下確認其資料

調整項目：
1.goTimer() 紀錄上班時間。
2.stopTimer() 停止計時。
3.workTimes() 計算總時長。
4.String data 回傳資料
5.timerSave() 紀錄資料
6.String load 讀取資料


========================================================================================
----------------------------------------------------------------------------------------

Controller

Class Items :
 //價格常數
	public final static int porkRice = 150;
	public final static int beefRice = 180;	
	public final static int lambRice = 200;
	
	//field 數量 總價格
	private static Integer prCount = 0;
	private static Integer brCount = 0;	
	private static Integer lrCount = 0;
	public static Integer sum;
	private static String outText;
	
	public static Integer prSum = 0;
	public static Integer brSum = 0;
	public static Integer lrSum = 0;
	
	//建構式 constructor
	
	
	//建構式 end constructor
	
	//method

	public static Integer getPrCount() {
		return prCount;
	}

	public static void setPrCount(Integer prCount) {
		Items.prCount = prCount;
	}

	public static Integer getBrCount() {
		return brCount;
	}

	public static void setBrCount(Integer brCount) {
		Items.brCount = brCount;
	}

	public static Integer getLrCount() {
		return lrCount;
	}

	public static void setLrCount(Integer lrCount) {
		Items.lrCount = lrCount;
	}

	public Integer getSum() {
		return sum;
	}

	public void setSum(Integer sum) {
		this.sum = sum;
	}

	public String getOutText() {
		return outText;
	}

	
	public static String outPut() {
		prSum = porkRice*prCount;
		brSum = beefRice*brCount;
		lrSum = lambRice*lrCount;
		sum = prSum +brSum+lrSum;
		if(prCount>0 && brCount>0 && lrCount>0) {
			outText = "豬排飯 : "+prCount+"份"+ prSum +"元"+
					"\n牛排飯 : " + brCount +"份"+brSum+"元"+
					"\n羊排飯 : "+ lrCount +"份"+lrSum+"元"
					+"\n\n\n\n\n\t  合計 : "+sum+"元";
			}else if(prCount==0 && brCount>0 && lrCount>0){
				outText = "牛排飯 : " + brCount +"份"+brSum+"元"+
					"\n羊排飯 : "+ lrCount +"份"+lrSum+"元"
					+"\n\n\n\n\n\n\t  合計 : "+sum+"元";
			}else if(prCount>0 && brCount>0 && lrCount==0){
				outText = "豬排飯 : "+prCount+"份"+ prSum +"元"+
						"\n牛排飯 : " + brCount +"份"+brSum+"元"
						+"\n\n\n\n\n\n\t  合計 : "+sum+"元";
			}else if(prCount>0 && brCount==0 && lrCount>0){
				outText = "豬排飯 : "+prCount+"份"+ prSum +"元"+
					"\n羊排飯 : "+ lrCount +"份"+lrSum+"元"
					+"\n\n\n\n\n\n\t  合計 : "+sum+"元";
			}else if(prCount==0 && brCount==0 && lrCount>0){
				outText ="羊排飯 : "+ lrCount +"份"+lrSum+"元"
						+"\n\n\n\n\n\n\n\t  合計 : "+sum+"元";
			}else if(prCount==0 && brCount>0 && lrCount==0){
				outText ="牛排飯 : " + brCount +"份"+brSum+"元"
						+"\n\n\n\n\n\n\n\t  合計 : "+sum+"元";
			}else if(prCount>0 && brCount==0 && lrCount==0){
				outText ="豬排飯 : "+prCount+"份"+ prSum +"元"
						+"\n\n\n\n\n\n\n\t  合計 : "+sum+"元";
			}else if(prCount==0 && brCount==0 && lrCount==0){
				outText ="";
			}
		return outText;		
	}
	
	public static String outPutVip() {
		prSum = porkRice*prCount*9/10;
		brSum = beefRice*brCount*9/10;
		lrSum = lambRice*lrCount*9/10;
		sum = prSum +brSum+lrSum;
		if(prCount>0 && brCount>0 && lrCount>0) {
			outText = "VIP 10% OFF \n"+"豬排飯 : "+prCount+"份"+ prSum +"元"+
					"\n牛排飯 : " + brCount +"份"+brSum+"元"+
					"\n羊排飯 : "+ lrCount +"份"+lrSum+"元"
					+"\n\n\n\n\t  合計 : "+sum+"元";
			}else if(prCount==0 && brCount>0 && lrCount>0){
				outText ="VIP 10% OFF \n"+ "牛排飯 : " + brCount +"份"+brSum+"元"+
					"\n羊排飯 : "+ lrCount +"份"+lrSum+"元"
					+"\n\n\n\n\n\t  合計 : "+sum+"元";
			}else if(prCount>0 && brCount>0 && lrCount==0){
				outText = "VIP 10% OFF \n"+"豬排飯 : "+prCount+"份"+ prSum +"元"+
						"\n牛排飯 : " + brCount +"份"+brSum+"元"
						+"\n\n\n\n\n\t  合計 : "+sum+"元";
			}else if(prCount>0 && brCount==0 && lrCount>0){
				outText = "VIP 10% OFF \n"+"豬排飯 : "+prCount+"份"+ prSum +"元"+
					"\n羊排飯 : "+ lrCount +"份"+lrSum+"元"
					+"\n\n\n\n\n\t  合計 : "+sum+"元";
			}else if(prCount==0 && brCount==0 && lrCount>0){
				outText ="VIP 10% OFF \n"+"羊排飯 : "+ lrCount +"份"+lrSum+"元"
						+"\n\n\n\n\n\n\t  合計 : "+sum+"元";
			}else if(prCount==0 && brCount>0 && lrCount==0){
				outText ="VIP 10% OFF \n"+"牛排飯 : " + brCount +"份"+brSum+"元"
						+"\n\n\n\n\n\n\t  合計 : "+sum+"元";
			}else if(prCount>0 && brCount==0 && lrCount==0){
				outText ="VIP 10% OFF \n"+"豬排飯 : "+prCount+"份"+ prSum +"元"
						+"\n\n\n\n\n\n\t  合計 : "+sum+"元";
			}else if(prCount==0 && brCount==0 && lrCount==0){
				outText ="";
			}
		return outText;		
	}
	
	public static String change(Integer pay,Integer sum) {
		Integer diff = pay - sum;
		String changeOutput = "";
		if (pay>=sum && diff >= 0) {
			int count1000 = 0;
			int count500 = 0;
			int count100 = 0;
			int count50 = 0;
			int count10 = 0;
			int count5 = 0;
			int count1 = 0;

			if (diff / 1000 >= 1) {
				count1000 = diff / 1000;
				diff = diff - count1000 * 1000;
			}
			if (diff / 500 >= 1) {
				count500 = diff / 500;
				diff = diff - count500 * 500;
			}
			if (diff / 100 >= 1) {
				count100 = diff / 100;
				diff = diff - count100 * 100;
			}
			if (diff / 50 >= 1) {
				count50 = diff / 50;
				diff = diff - count50 * 50;
			}
			if (diff / 10 >= 1) {
				count10 = diff / 10;
				diff = diff - count10 * 10;
			}
			if (diff / 5 >= 1) {
				count5 = diff / 5;
				diff = diff - count5 * 5;
			}
			if (diff / 1 >= 1) {
				count1 = diff / 1;
				diff = diff - count1;
			}
			
			changeOutput = ("實收" + pay + "  總金額" + sum + "\n須找零:" + (pay - sum)
					+ "\n1000元 " + count1000 + "張" + "\n500元 " + count500 + "張" + "\n100元 " + count100 + "張"
					+ "\n50元 " + count50 + "個" + "\n10元 " + count10 + "個" + "\n5元 " + count5 + "個" + "\n1元 "
					+ count1 + "個");
		}
		return changeOutput;
	}
	//method end
}


----------------------------------------------------------------------------------------
========================================================================================

========================================================================================
----------------------------------------------------------------------------------------

DaO
Class DateTimer:
public static String getTime() {
		Calendar calendar = Calendar.getInstance();
		int year = calendar.get(Calendar.YEAR);
		int month = calendar.get(Calendar.MONTH) + 1;
		int day = calendar.get(Calendar.DATE);
		int hour = calendar.get(Calendar.HOUR_OF_DAY);
		int minute = calendar.get(Calendar.MINUTE);
		int second = calendar.get(Calendar.SECOND);
		return (year + "-" + String.format("%02d", month) + "-" + String.format("%02d", day) + " "
				+ String.format("%02d", hour) + "." + String.format("%02d", minute) + "." +
				String.format("%02d", second));
	}

----------------------------------------------------------------------------------------

Class OutputExcel:
public static Integer order= 1;
	public static String fileNameIn= "";
	public static String dateMsgIn= "";
	public static String empolyeeMsgIn= "";
	public static Integer sumMsgIn= 0;
	public static Integer payMsgIn= 0;
	public static Integer changeMsgIn= 0;
	public static String vipMsgIn= "";
	public static Integer countPrIn= 0;
	public static Integer countBrIn= 0;
	public static Integer countLrIn= 0;
	public static Integer itpricePrIn= 0;
	public static Integer itpriceBrIn= 0;
	public static Integer itpriceLrIn= 0;
	public static Integer totleMsgIn= 0;
	
	public static void createExcel(String fileNameIn,String dateMsgIn,String empolyeeMsgIn,Integer sumMsgIn,
			Integer payMsgIn,Integer changeMsgIn,String vipMsgIn,Integer countPrIn,Integer countBrIn,Integer countLrIn,
			Integer itpricePrIn,Integer itpriceBrIn,Integer itpriceLrIn,Integer totleMsgIn) throws IOException {
		 Workbook workbook = new XSSFWorkbook();
	        Sheet sheet = workbook.createSheet("Sheet1");
	        Row [] row = new Row[15];
	        for(int i = 0;i<row.length;i++) {
	        	row[i] = sheet.createRow(i);
	        }
	        sheet.setColumnWidth(1, 256*10);
	        
	        Cell date = row[1].createCell(1); //createCell 橫排位置
	        Cell dateMsg = row[1].createCell(2);
	        Cell empolyee = row[2].createCell(1);
	        Cell empolyeeMsg = row[2].createCell(2);
	        Cell sum = row[3].createCell(1);
	        Cell sumMsg = row[3].createCell(2);
	        Cell pay = row[4].createCell(1);
	        Cell payMsg = row[4].createCell(2);
	        Cell change = row[5].createCell(1);
	        Cell changeMsg = row[5].createCell(2);
	        Cell vip = row[6].createCell(1);
	        Cell vipMsg = row[6].createCell(2);
	        Cell items = row[7].createCell(1);	 
	        Cell itemPr = row[8].createCell(1);
	        Cell itemBr = row[9].createCell(1);
	        Cell itemLr = row[10].createCell(1);
	        Cell count = row[7].createCell(2);
	        Cell countPr = row[8].createCell(2);
	        Cell countBr = row[9].createCell(2);
	        Cell countLr = row[10].createCell(2);
	        Cell price = row[7].createCell(3);
	        Cell pricePr = row[8].createCell(3);
	        Cell priceBr = row[9].createCell(3);
	        Cell priceLr = row[10].createCell(3);
	        Cell itPrice = row[7].createCell(4);
	        Cell itPricePr = row[8].createCell(4);
	        Cell itPriceBr = row[9].createCell(4);
	        Cell itPriceLr = row[10].createCell(4);
	        Cell totle = row[11].createCell(1);
	        Cell totleMsg = row[11].createCell(4);
	        
	        date.setCellValue("日期:");
	        dateMsg.setCellValue(dateMsgIn);
	        empolyee.setCellValue("結帳人員:");
	        empolyeeMsg.setCellValue(empolyeeMsgIn);
	        sum.setCellValue("總金額:");
	        sumMsg.setCellValue(sumMsgIn);
	        pay.setCellValue("實收金額");
	        payMsg.setCellValue(payMsgIn);
	        change.setCellValue("找零");
	        changeMsg.setCellValue(changeMsgIn);
	        items.setCellValue("品項:");
	        vip.setCellValue("VIP(Y/N)");
	        vipMsg.setCellValue(vipMsgIn);
	        items.setCellValue("品項");
	        itemPr.setCellValue("豬排飯");
	        itemBr.setCellValue("牛排飯");
	        itemLr.setCellValue("羊排飯");
	        count.setCellValue("份數");
	        countPr.setCellValue(countPrIn);
	        countBr.setCellValue(countBrIn);
	        countLr.setCellValue(countLrIn);
	        price.setCellValue("單價(元)");
	        pricePr.setCellValue("150");
	        priceBr.setCellValue("180");
	        priceLr.setCellValue("200");
	        itPrice.setCellValue("合計(元)");
	        itPricePr.setCellValue(itpricePrIn);
	        itPriceBr.setCellValue(itpriceBrIn);
	        itPriceLr.setCellValue(itpriceLrIn);
	        totle.setCellValue("總計");
	        totleMsg.setCellValue(sumMsgIn);
	        
	      
	        FileOutputStream outputStream = new FileOutputStream("orders(excel)/"+"訂單"+order+" "+fileNameIn+".xlsx");
	        workbook.write(outputStream);
	        outputStream.close();
	        order++;
----------------------------------------------------------------------------------------
Class Sign:
public static ArrayList<String> username = new ArrayList<>();
		public static ArrayList<String> password = new ArrayList<>();
		public static boolean tf;
		
		
		public static void addUser(String username,String password) throws IOException {
			Sign.username.add(username);
			Sign.password.add(password);
			FileOutputStream fos = new FileOutputStream("userData/empolyees.txt");
			ObjectOutputStream oos = new ObjectOutputStream(fos);
			oos.writeObject(Sign.username);
			oos.writeObject(Sign.password);
		}
		
		public static void read() throws IOException {
			FileInputStream fis = new FileInputStream("userData/empolyees.txt");
			ObjectInputStream ois = new ObjectInputStream(fis);			
			while (true) {
				try {
					Sign.username = (ArrayList<String>) ois.readObject() ;
					Sign.password = (ArrayList<String>) ois.readObject() ;
					
				} catch (Exception e) {
					break;
				}
			}
		}

		public static boolean checkUsername(String username) {
			boolean bool = true;
			for (int i = 0; i < Sign.username.size(); i++) {
				if (Sign.username.get(i).equals(username)) {
					bool = false;
				}
			}
				return bool;
			}
		
		public static boolean check(String nm,String pw) {
			
			if (Sign.username.indexOf(nm)>=0&& 
					Sign.password.get(Sign.username.indexOf(nm)).equals(pw)){
				
				Sign.tf=true;
			}else {
				
				Sign.tf=false;
			}
			return tf;
		}
----------------------------------------------------------------------------------------
Class WorkTimer:

public static Integer sec = 0;
	public static Integer min = 0;
	public static Integer hr = 0;
	private static Timer timer;
	
	public static void goTimer() {
		timer = new Timer();
		timer.schedule(new TimerTask() {
			public void run() {
				sec++;
			}
		}, 0, 1000);
		
		
	}
	 public static void stopTimer(){
	        timer.cancel();
	    }
	 
	 public static String workTimes(){
		 if(sec>=60) {
			 min = sec/60;
			 sec = sec%60;
		 }
		 if(min>=60) {
			 hr = min/60;
			 min = min%60;
		 }
		return hr+" hr "+min+" min "+ sec+" sec ";
		 
	 }
	 public static String data(String employName,String goTime,
			 String homeTime,String sumTimes) {
		 return "員工 : "+employName+"\n 上班時間 : "+goTime+
				 "\n 下班時間 : "+homeTime+"\n 值機時長 : "+sumTimes;
	 }
	 
	 public static void timerSave(String empolyName,String date,String timeData) throws IOException {
		date = date.substring(0, 10);
		FileOutputStream fos = new FileOutputStream("empolyeesData/"+empolyName+" "+date+".txt");
		OutputStreamWriter osw = new OutputStreamWriter(fos);
		BufferedWriter bw = new BufferedWriter(osw);
		bw.write('\ufeff');
		bw.write(timeData);
		bw.close();
	}
	 
	public static String load(String name) throws IOException {
		String data = "";
		String addData = "";
		BufferedReader br = 
				new BufferedReader(
						new InputStreamReader(
								new FileInputStream(name)));

		while ((addData=br.readLine())!=null) {
			data = data+addData+"\n";
		}
		return data;
	}
----------------------------------------------------------------------------------------
========================================================================================


========================================================================================
----------------------------------------------------------------------------------------

Model
Class PadUI:
private JPanel contentPane;
	private static JTextField inputName;
	private JPasswordField inputPassword;
	public static String goWorkTime = "";
	
	public static JTextField getInputName() {
		return inputName;
	}
	
public void actionPerformed(ActionEvent e) {
				try {
					Sign.read();
				} catch (IOException e1) {
					// TODO Auto-generated catch block
					e1.printStackTrace();
				}			
				String pwd =  String.valueOf(inputPassword.getPassword());		
				
				if(Sign.check(getInputName().getText(), pwd)) {
			
					goWorkTime = timeArea1.getText();
					WorkTimer.goTimer();
					PadUI2 frame = new PadUI2();
					frame.setVisible(true);
					frame.setResizable(false);
					dispose();
				}else {
					errPanel.setVisible(true);
				}
				
			}
		});

public void actionPerformed(ActionEvent e) {
				dispose();
			}

public void actionPerformed(ActionEvent e) {
				signPage frame = new signPage();
				frame.setVisible(true);
				frame.setResizable(false);
			}


----------------------------------------------------------------------------------------
Class PadUI2:
public static String goHomeTime = "";
	private JPanel contentPane;
	private JTextField payField;	
	private String vipYN ="";

vipCheckBox.addActionListener(new ActionListener() {
			public void actionPerformed(ActionEvent e) {

				
				if (vipCheckBox.isSelected()) {
					countOutput.setText(Items.outPutVip());
					vipYN = "Y";
				} else {
					countOutput.setText(Items.outPut());
					vipYN = "";
				}
			}

		});

pRAddButton.addActionListener(new ActionListener() {
			public void actionPerformed(ActionEvent e) {
				Items.setPrCount(Items.getPrCount()+1);
				pRitems.setText(Items.getPrCount().toString());
				if(vipCheckBox.isSelected()) {
					countOutput.setText(Items.outPutVip());
				}else {
					countOutput.setText(Items.outPut());
				}
			}
		});

pRRudButton_1.addActionListener(new ActionListener() {
			public void actionPerformed(ActionEvent e) {
			
				if (Items.getPrCount() > 0) {
					Items.setPrCount(Items.getPrCount()-1);
				}
				pRitems.setText(Items.getPrCount().toString());
				if(vipCheckBox.isSelected()) {
					countOutput.setText(Items.outPutVip());
				}else {
					countOutput.setText(Items.outPut());
				}
			}
		});

clearButton.addActionListener(new ActionListener() {
			public void actionPerformed(ActionEvent e) {
				Items.setPrCount(0);
				Items.setBrCount(0);
				Items.setLrCount(0);
				
				pRitems.setText("");
				bRitems_1.setText("");
				lRitems_2.setText("");
				
				countOutput.setText("");
				payField.setText("");
				changeArea.setText("");
				billArea.setText("");
				vipCheckBox.setSelected(false);
				vipYN = "";
			}
		});

public void actionPerformed(ActionEvent e) {
				Integer pay = 0;
										
				if(payField.getText().length()==0) {
					changeArea.setText("請輸入收款金額");
				}else if((pay= Integer.parseInt(payField.getText()))<Items.sum) {
					changeArea.setText("收款金額小於結帳金額");
				}else {
					pay = Integer.parseInt(payField.getText());
					changeArea.setText(Items.change(pay,Items.sum));
					if (vipCheckBox.isSelected()) {
						billArea.setText("日期 : " + timeArea.getText() + "\n" + empolyLabel.getText() 
						+ "\n總金額 : " + Items.sum +
						"\n實收金額 : "+ pay+ "\n找零 : "+ (pay-Items.sum)
						+ "\n品項 : \n" + Items.outPutVip());
					} else {
						billArea.setText("日期 : " + timeArea.getText() + "\n" + empolyLabel.getText() 
						+ "\n總金額 : " + Items.sum +
						"\n實收金額 : "+ pay+ "\n找零 : "+ (pay-Items.sum)
						+"\n品項 : \n" + Items.outPut());
					}
				}
				
			}
		});

changeButton.addActionListener(new ActionListener() {
			public void actionPerformed(ActionEvent e) {
				goHomeTime = timeArea.getText();
				WorkTimer.stopTimer();
				workTime frame = new workTime();
				frame.setVisible(true);
				frame.setResizable(false);
				dispose();
				clearButton.doClick();
			}
		});

public void actionPerformed(ActionEvent e) {
				try {
					billArea.print();
				} catch (PrinterException e1) {
					// TODO Auto-generated catch block
					e1.printStackTrace();
				}
			}
		});

excelButton.addActionListener(new ActionListener() {
			public void actionPerformed(ActionEvent e) {
				if(payField.getText().equals("")||changeArea.getText().equals("")) {
					excelLabel.setText("請確認訂單");
					excelLabel.setVisible(true);
					
				}else {						
					try {
						OutputExcel.createExcel(
								timeArea.getText(), timeArea.getText(), PadUI.getInputName().getText(),
								Items.sum, (Integer.parseInt(payField.getText())), 
								((Integer.parseInt(payField.getText()))-Items.sum), 
								vipYN, Items.getPrCount(), Items.getBrCount(), Items.getLrCount(), 
								Items.prSum, Items.brSum, Items.lrSum, Items.sum);
						excelLabel.setText("匯出成功");
						excelLabel.setVisible(true);
						
						
					} catch (IOException e1) {
						// TODO Auto-generated catch block						
						e1.printStackTrace();				
					}
				}
			}
		});


----------------------------------------------------------------------------------------
Class signPage:
private JPanel contentPane;
	private JTextField inUsername;
	private JTextField inPwd;

sign.addActionListener(new ActionListener() {
			public void actionPerformed(ActionEvent e) {
				try {
					Sign.read();
				} catch (IOException e2) {
					// TODO Auto-generated catch block
					e2.printStackTrace();
				}
				Boolean check =	Sign.checkUsername(inUsername.getText());
				if(check&&inUsername.getText().length()==4&&inPwd.getText().length()==4) {
					try {
						System.out.println("註冊前"+Sign.username);
						System.out.println("註冊後"+Sign.password);
						Sign.read();
						Sign.addUser(inUsername.getText(), inPwd.getText());
						success.setText("註冊成功，請關閉視窗");
						System.out.println(Sign.username);
						System.out.println(Sign.password);
					} catch (IOException e1) {
						// TODO Auto-generated catch block
						e1.printStackTrace();
					}
				}else if(check==false) {
					success.setText("註冊失敗，Username重複");
				}
				else {
					success.setText("註冊失敗，請確認長度為4");
				}
			}
		});



----------------------------------------------------------------------------------------
Class WorkTime:
downSaveBtn.addActionListener(new ActionListener() {
			public void actionPerformed(ActionEvent e) {
				String data = WorkTimer.data(PadUI.getInputName().getText(),
						PadUI.goWorkTime, PadUI2.goHomeTime, 
						WorkTimer.workTimes());
				
				try {
					WorkTimer.timerSave(PadUI.getInputName().getText(),
							PadUI2.goHomeTime,data);
					success.setText("存檔成功 檔名 "+PadUI.getInputName().getText()+
							" "+PadUI2.goHomeTime.substring(0, 10)+".txt");
					success.setVisible(true);
				} catch (IOException e1) {
					// TODO Auto-generated catch block
					e1.printStackTrace();
				}
			}
		});

loadBtn.addActionListener(new ActionListener() {
			public void actionPerformed(ActionEvent e) {
				
				try {
					loadArea.setText(WorkTimer.load("empolyeesData/"+
					PadUI.getInputName().getText()+" "+PadUI2.goHomeTime.substring(0, 10)+".txt"));
				} catch (IOException e1) {
					// TODO Auto-generated catch block
					e1.printStackTrace();
				}
			}
		});

----------------------------------------------------------------------------------------
========================================================================================