1. ��������������Ӧ�ó�����
	ϵͳ�д���������̫��������һ��4g����ģ�����̫�������ټ�һ����̫��������һ������Internet���ʣ�
��һ�����ھ���������(Ŀǰֻ֧��DHCP��̬��ʽ��ȡIP��ַ)��

2. ��δ򲹶���
	�����Android 5.x��6.0ƽ̨����frameworks/opt/net/ethernet/Ŀ¼�´���Android 5.x & 6.0/1.diff����
	Ȼ���ٽ�Android 5.x & 6.0/EthernetNetworkFactoryExt.java������frameworks/opt/net/ethernet/java/com/android/server/ethernet$Ŀ¼��

	�����Android 7.xƽ̨����frameworks/opt/net/ethernet/Ŀ¼�´���Android 7.x/1.diff����
	Ȼ���ٽ�Android 7.x/EthernetNetworkFactoryExt.java������frameworks/opt/net/ethernet/java/com/android/server/ethernet$Ŀ¼��

        ��init.rockchip.rc����ӷ���
        service dhcpcd_eth1 /system/bin/dhcpcd -aABDKL
             class late_start
             disabled
             oneshot		

3. ��α�����Ч��
	mmm frameworks/opt/net/ethernet/�����ɵ�ethernet-service.jar���µ���������Ч

4. Ŀǰ����Ĭ���������³�����
	��������������һ����eth0�����ڷ���Internet����һ����eth1�����ڷ��ʾ�����
	ע�⣺���ϵͳ�д���������̫����������һ����gmac����һ����usb ethernet)���������������Ǹ�������ע���eth0����������������ע���eth1
	���Ҫ�̶�gmac��eth1, usb ethernetΪeth0���ɰ��ο�delay_start_of_gmac_driver.diff�޸ģ���gmac��������usb ethernet����

	eth0Ĭ�����ڷ���Internet��eth1Ĭ�����ڷ��ʾ����������Ҫ�޸ĳ�usb0������4g��Ĭ�����ڷ���Internet��eth0(��̫����)���ڷ��ʾ�����
	��Ҫ�޸�frameworks/opt/net/ethernet/java/com/android/server/ethernet/EthernetNetworkFactory.java�е�mIfaceMatch = "eth0";ΪmIfaceMatch = "usb0";
		�޸�frameworks/opt/net/ethernet/java/com/android/server/ethernet/EthernetNetworkFactoryExt.java�е�mIface = "eth1";ΪmIface = "eth0";

5. ��̫��eth1ģʽ�����֣�
     
        1��DHCP                    ��̬��ȡIP��ַ
        2��STATIC                  ��̬IP��ַ
        3��STATIC+DHCPSERVER       ��̬IP��ַ��������DHCPSERVER�����Ը������豸����IP��ַ

   Ĭ��Ϊ��̬��ȡ��ַ�����ر�dhcpserver����ͨ����������Ϊ����ģʽ

        1��persist.net.eth1.mode      0��dhcp    1��static
        2��persist.dhcpserver.enable  0���ر�dhcpserver   1������dhcpserver              ע�⣺dhcpserverֻ����staticģʽ�¿�������Ч

   staticģʽ����Ӿ�̬��ַ��Ϣ���ԣ�������ʾ������Ϊip��netmask��ֻ���ھ���������˲���Ҫ���غ�dns��

       persist.net.eth1.staticinfo  192.168.1.100��24
       ע�⣺ip��netmask�����ö��ŷָû�пո񣩣�netmaskΪ���볤��int��

   ������dhcpserverģʽ��������dhcpserver�ĵ�ַ�أ�������IP��ַ�ķ�Χ�����뾲̬IP��ַ����һ���Ҳ���ͻ

      persist.dhcpserver.start 192.168.1.150
      persist.dhcpserver.end 192.168.1.250