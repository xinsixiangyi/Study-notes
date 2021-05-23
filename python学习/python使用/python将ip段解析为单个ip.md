## 将ip段解析为单个ip

将类似于192.168.1.* 192.168.1.0-255 192.168.1-10.* 192.168.1.0/24 这样的段IP解析为单个的ip，以方便后续的扫描等操作。

### 192.168.1.*样式的ip段分解

```python

def IPSplitStar(ips):               # 192.168.1.*  --->   192.168.1.1   到    192.168.1.255
	ip_1 = ips.split('.')[-4]
	ip_2 = ips.split('.')[-3]
	ip_3 = ips.split('.')[-2]
	ip_4 = ips.split('.')[-1]
	ip_to = []
	for i in range(1,256):
    	ip_result = ip_1 + '.' + ip_2 + '.' + ip_3 + '.' + str(i)
    	ip_to.append(ip_result)
	return ip_to
```

以’.'为分隔符，来获取各个位置上的数据，然后*用1-255来替换。

### 192.168.1-10.*样式的ip段进行分解

```python
  
  def IPSplit_Star(ips):   #192.168.1-10.*
		ip_1 = ips.split('-')[-2].split('.')[-3]
		ip_2 = ips.split('-')[-2].split('.')[-2]
		ip_3 = ips.split('-')[-2].split('.')[-1]  #1
		ip_last = ips.split('-')[-1]  #10
		ip_last_1 = ip_last.split('.')[-2]
		ip_3p = []
		for i in range(int(ip_3),int(ip_last_1)+1):
    		for j in range(1,256):
        		ip_3p.append(ip_1 + '.' + ip_2 + '.' + str(i)+ '.' + str(j))
		return ip_3p

```

先以‘-’进行分割，然后再对‘-’前面的部分以‘.’分割，获取各个部分。获取‘-‘前后的两个数字作为外层循环的循环条件，里层循环是从1-255。最终解析为：192.168.1.0 192.168.1.1 ~~ 192.168.10.255

### 192.168.1.0-255样式的ip段进行分解

```python
def IPSplit_S(line):    #192.168.1.0-255
    dic = []
    ip_1 = line.split('-')[-2].split('.')[-4]
    ip_2 = line.split('-')[-2].split('.')[-3]
    ip_3 = line.split('-')[-2].split('.')[-2]
    ip_4 = line.split('-')[-2].split('.')[-1]
    ip_last = line.split('-')[-1]
    ip_last.strip()
    ip_len = int(ip_last) - int(ip_4) + 1
    for i in range(ip_len):        ip_last = int(ip_4) + i
        ip_result = ip_1 + '.' + ip_2 + '.' + ip_3 + '.' + str(ip_last)
        dic.append(ip_result)
        return dic
```


先以’-‘进行分割，取’-‘前面的部分再以’.‘分割，获取各部分的数据。获取’-'前后的数据求得中间的距离，然后作为循环条件，最终将12.168.1.0-255样式的IP段分解为单个ip。

### 192.168.1.0/24样式的ip段进行分解

```python
	from IPy import IP
	def IPSplit_d(line):    #192.168.1.0-255
		    dic = []
		    ip = IP(line)
		    for x in ip:
		    	dic.append(str(x))
            return dic
```

IPy 模块是python专门用来处理IP地址的。IP()方法是输出指定网段的IP清单。

### 192.168.1.250-192.168.2.5样式的ip段进行分解



	def IPSplitBlocks(ip2ip_str):
		ipx = ip2ip_str.split('-')ip2num = lambda x:sum([256**i*int(j) for i,j in enumerate(x.split('.')[::-1])])
	
	num2ip = lambda x: '.'.join([str(x//(256**i)%256) for i in range(3,-1,-1)])
	
	a = [num2ip(i) for i in range(ip2num(ipx[0]),ip2num(ipx[1])+1) if not ((i+1)%256 == 0 or (i)%256 == 0)]
	
	print (a)
	return a
解为：192.168.1.250 192.168.1.251 192.168.1.252 192.168.1.253 192.168.1.254 192.168.1.255 192.168.2.1 192.168.2.2 192.168.2.3 192.168.2.4 192.168.2.5

整体代码附上
	

	
	if __name__ == '__main__':from IPy import IP
	import re,os,sys,time
	def IPSplitStar(ips):  # 192.168.1.*  --->   192.168.1.1   到    192.168.1.255
		ip_1 = ips.split('.')[-4]
		ip_2 = ips.split('.')[-3]
		ip_3 = ips.split('.')[-2]
		ip_4 = ips.split('.')[-1]
		ip_to = []
		for i in range(1,256):
			ip_result = ip_1 + '.' + ip_2 + '.' + ip_3 + '.' + str(i)
			ip_to.append(ip_result)
		return ip_to
	
	
	def IPSplit_Star(ips):   #192.168.1-10.*
		ip_1 = ips.split('-')[-2].split('.')[-3]
		ip_2 = ips.split('-')[-2].split('.')[-2]
		ip_3 = ips.split('-')[-2].split('.')[-1]  #1
		ip_last = ips.split('-')[-1]  #10
		ip_last_1 = ip_last.split('.')[-2]
		ip_3p = []
		for i in range(int(ip_3),int(ip_last_1)+1):
			for j in range(1,256):
				ip_3p.append(ip_1 + '.' + ip_2 + '.' + str(i)+ '.' + str(j))
		return ip_3p
	
	def IPSplit(ip2ip_str):
		dic = []
		dic1 = []
		#filename = 'IP-range.txt'       #IP segment filename to be processed
		#fo = open(filename,"r")
		for line in ip2ip_str:#for line in fo:
			data = line
			ips = re.search(r'((2[0-4]\d|25[0-5]|[01]{0,1}\d{0,1}\d)\.){3}(2[0-4]\d|25[0-5]|[01]{0,1}\d{0,1}\d)[-/]',line)      #Select the network segment type
			if(ips != None):
				ips = re.search(r'((2[0-4]\d|25[0-5]|[01]{0,1}\d{0,1}\d)\.){3}(2[0-4]\d|25[0-5]|[01]{0,1}\d{0,1}\d)[/]',line)   #Separate/type and -type		
			#Processing/type
			if(ips != None):
				ip = IP(data)
				for x in ip:
					dic.append(str(x))
			
			#process-type        
			else:
				ip_1 = line.split('-')[-2].split('.')[-4]
				ip_2 = line.split('-')[-2].split('.')[-3]
				ip_3 = line.split('-')[-2].split('.')[-2]
				ip_4 = line.split('-')[-2].split('.')[-1]
				ip_last = line.split('-')[-1]
				ip_last.strip()
				ip_len = int(ip_last) - int(ip_4) + 1
				for i in range(ip_len):
					
					ip_last = int(ip_4) + i
					ip_result = ip_1 + '.' + ip_2 + '.' + ip_3 + '.' + str(ip_last)
					dic.append(ip_result)
		else:
			#dic.append(data)
			if '*' in line and '-' in line:
				dic = dic + IPSplit_Star(line)
			elif '*' in line:
				dic = dic + IPSplitStar(line)
			else:
				dic.append(line)
	#print '查看结果  开始'
	#print dic
	#print '查看结果  结束'
	return list(set(dic))   #目的是为了去重
	if __name__ == '__main__':
	    #dic = IPSplit(['192.168.1.1-24','192.168.1-2.*'])
	    dic = IPSplit(['192.168.1.1'])
	    print dic
	    print len(dic)
	    #print IPSplitStar('192.168.3.*')
	    #print IPSplit_Star('192.168.1-2.*') #192.168.1-10.*  :  

附：
r’((2[0-4]\d|25[0-5]|[01]{0,1}\d{0,1}\d).){3}(2[0-4]\d|25[0-5]|[01]{0,1}\d{0,1}\d)[-/]’ 是区分IP段中是否含有-以及/

r’((2[0-4]\d|25[0-5]|[01]{0,1}\d{0,1}\d).){3}(2[0-4]\d|25[0-5]|[01]{0,1}\d{0,1}\d)[/] 是区分IP段中是否含有/

r'(([01]{0,1}\d{0,1}\d|2[0-4]\d|25[0-5])\.){3}([01]{0,1}\d{0,1}\d|2[0-4]\d|25[0-5])'   匹配是否存在ip

