#	wxshare.js��ʹ�÷���

### wxshare.js����΢�ŷ�����ģ�������� `wx`��`auth`��`ngapi`ģ�顣

> ��require�����д�ģ������ô���Ϊ `wxshare` 

#	API

### wxshare.initWx(share, letwxid, apiopenid, apitoken, succCb, cancelCb, errorCb)
΢�ŷ���
* share	: ���룬share��һ��json��ʽ�Ķ��󣬸�ʽ����
	```
	{
		title:'title',
		desc:'desc',
		link:'link',
		imgUrl:'imgUrl'
	}
	```
	* title : ����ı���
	* desc : ���������
	* link : ���������
	* imgUrl : �����ͼ��
* letwxid �� ���룬 ��Ӧ�˺ŵ�letwxid
* apiopenid �� ���룬�û���apiopenid
* apitoken ��  ���룬�û���apitoken
* succCb �� ��ѡ�� ����ɹ��Ļص�����
* cancelCb �� ��ѡ�� ȡ������Ļص�����
* errorCb : ��ѡ������ʧ�ܵĻص�����

### wxshare.initWxAuth(share,letwxid,succCb,cancelCb, errorCb)
΢�ŷ����˷����� `wxshare.initWx` �����������ڴ˷����ڲ������auth��Ȩ����ȥ�û��� `apiopenid` �� `apitoken`���� `wxshare.initWx`������м�Ȩ����Ϊ	 `wxshare.initWx`�Ĳ����д������û���Ϣ

�˷����еĸ������� `wxshare.initWx`��һ�¡�

### wxshare.initWxCfg(share, wxconfig, succCb, cancelCb, errorCb)
����ע��΢�ŷ����¼���
>һ������£��˷���ֻ����wxshare.js�ڲ�ʹ�ã�������Ҫ�ⲿ���á�

* share : �������Ϣ��ͬ�ϡ�
* wxconfig : wx.config��������Ϣ
* succCb �� ��ѡ�� ����ɹ��Ļص�����
* cancelCb �� ��ѡ�� ȡ������Ļص�����
* errorCb : ��ѡ������ʧ�ܵĻص�����

���˽�����й�΢��JS-SDK�������Ϣ����ο���[΢��JS-SDK�ٷ��ĵ�](http://mp.weixin.qq.com/wiki/7/aaa137b55fb2e0456bf8dd9148dd613f.html)