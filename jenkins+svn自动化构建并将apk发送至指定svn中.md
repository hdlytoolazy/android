��˾����д���Զ�����Ҫ�󣬽�����ı��롢����Լ�������svn��Ҳ������git����Щ��ʱ�����Ĳ����ŵ��������ϣ�����ߣ����ķ��㣬���Ե�ͬ��Ҳ���㣬

���ԾͿ�ʼ����


һ��Jenkins+Gradleʵ��android�����������ɡ����

Jenkins�İ�װ���á��Լ�Gradle�Ļ�����������Ͳ��ٹ���������ˣ��������£�

http://blog.csdn.net/xiongmc/article/details/26515577


����������͵�ָ��svnĿ¼��

���������������

1��������apkҪ�ŵ�svnĿ¼���Ǵ������ڵ�svnĿ¼

���������ͬѧ��ϲ�㣬������������߾Ϳ��Ը㶨��

https://www.cnblogs.com/EasonJim/p/6828364.html

2��������apkҪ�ŵ�һ��ר�ŵ�svn·���У�������Ϊʲô��������xx˵��Ҫ�淶...��

�� �Ƚ�����checkout����������ĳ��·���£�ע���ⲻ�Ƿϻ�������Ҫ�����·����������

�� ���ķ���������Ŀ��Gradle�ļ���Ŀ���ǽ��������ļ�ֱ��������ٵ�·���У�������£�

android{

	applicationVariants.all { variant ->
	
        variant.outputs.each { output ->
		
            def today = new Date().format('MMdd-HHmm');//�Զ����������
			
            def outputFile = output.outputFile;//def��Ϊ���壬��ʵ����԰�������String outputFile = output.outputFile���������
			
            def newFilePath = "/data/jenkinsPack/version3.x.x_test";
			
            if (outputFile != null && outputFile.name.endsWith('.apk')) {
			
                def oldFileName = outputFile.name.replace(".apk","-" + today + "-" + defaultConfig.versionName + ".apk");//replace��������ԭ����xxx-release.apk����Ϊxxx-release-0802-1212-v1.0.0.apk������apk����
				
                File newFile = new File(newFilePath);//ָ���ƶ�����Ŀ��Ŀ¼
				
                output.outputFile = new File(newFile,oldFileName );//����������Ŀ��Ŀ¼
				
//               outputFile.delete()

            }else {
			
            }
			
        }
		
    }
	
}


ע�⣺newFilePath��·��һ��Ҫ��Ӧ����svn��·����ע��·������ȷ���Լ��Ƿ��ж�дȨ�ޣ���δ����Ƿ���android{}��ġ�

�� ��һ�������ʹ��SVN Publisher��������ֱ�ӽ������ڹ�������µ�svn�������ھ�ֻ��дshell�ű��ˣ��������ͼ��

![Alt text](https://raw.githubusercontent.com/hdlytoolazy/android/master/raw/Screenshots/20180126215830.png)

--no-auth-cache  ��������֤��Ϣ������������������ͼ�Ĵ���

![Alt text](https://raw.githubusercontent.com/hdlytoolazy/android/master/raw/Screenshots/20180126221714.png)

svn st | grep '^\?' | tr '^\?' ' ' | sed 's/[ ]*//' | sed 's/[ ]/\\ /g' | xargs svn add  ֱ��add����δ��ӹ����ļ�or�ļ���

ע�⣺����ʹ����ʱ�û�ֱ���ύ���ܲ��Ǻܺã����ڿ����Ż�һ�½ű���ʹ���Ѿ����б��е��û����в�����


������������ĵ�ȥJenkins�Ϲ����ɣ�
