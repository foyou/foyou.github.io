---
layout:        post
title:         图片上传到ftp指定文件夹
description:   "图片上传到ftp指定文件夹的功能，c#版本的实现起来也比较简单，可以一次上传多张图片。"
tags: [Upload]
image:
  background: body_bg.gif
comments: true
share: true
---

图片上传到ftp指定文件夹的功能，c#版本的实现起来也比较简单，可以一次上传多张图片。
    
<!--more-->

{% highlight JavaScript %}
        #region 文件上传Ftp
        #region 公共字段
        public readonly static  string targetDir = "/";  //文档目录
        public readonly static string hostname = "127.0.0.1:21"; //IP
        public readonly static string username = "mcalvin"; //用户名
        public readonly static string password = "xx"; //密码
        #endregion
     
        /// <summary>
        /// 上传图片
        /// </summary>
        private void UploadImg()
        {
            this.openFileDia.Filter = "jpg文件(*.jpg)|*.jpg|gif文件(*.gif)|*.gif|png文件(*.png)|*.png";
            openFileDia.Multiselect = true;
            FileInfo FILE = null;
            if (this.openFileDia.ShowDialog() == DialogResult.OK)
            {
                string[] PicFileName = this.openFileDia.FileNames;
 
                foreach (var filename in PicFileName)
                {
                    FILE = new FileInfo(filename);
                    if (!ftpIsExistsFile(targetDir + "/" + DateTime.Now.ToString("yyyy-MM-dd"), hostname, username, password))
                        MakeDir(targetDir + "/" + DateTime.Now.ToString("yyyy-MM-dd"), hostname, username, password);
                    UploadFile(FILE, targetDir + "/" + DateTime.Now.ToString("yyyy-MM-dd"), hostname, username, password);
                }
            }
        }
        /// <summary>
        /// 上传文件
        /// </summary>
        /// <param name="fileinfo">需要上传的文件</param>
        /// <param name="targetDir">目标路径</param>
        /// <param name="hostname">ftp地址</param>
        /// <param name="username">ftp用户名</param>
        /// <param name="password">ftp密码</param>
        public static void UploadFile(FileInfo fileinfo, string targetDir, string hostname, string username, string password)
        {
            //1. check target
            string target;
            if (targetDir.Trim() == "")
            {
                return;
            }
            target = Guid.NewGuid().ToString();  //使用临时文件名
            string URI = "FTP://" + hostname + "/" + targetDir + "/" + target+fileinfo.Extension;
            ///WebClient webcl = new WebClient();
            System.Net.FtpWebRequest ftp = GetRequest(URI, username, password);
 
            //设置FTP命令 设置所要执行的FTP命令，
            //ftp.Method = System.Net.WebRequestMethods.Ftp.ListDirectoryDetails;//假设此处为显示指定路径下的文件列表
            ftp.Method = System.Net.WebRequestMethods.Ftp.UploadFile;
            //指定文件传输的数据类型
            ftp.UseBinary = true;
            ftp.UsePassive = true;
             
            //告诉ftp文件大小
            ftp.ContentLength = fileinfo.Length;
            //缓冲大小设置为2KB
            const int BufferSize = 2048;
            byte[] content = new byte[BufferSize];
            int dataRead;
 
            //打开一个文件流 (System.IO.FileStream) 去读上传的文件
            using (FileStream fs = fileinfo.OpenRead())
            {
                try
                {
                    //把上传的文件写入流
                    using (Stream rs = ftp.GetRequestStream())
                    {
                        do
                        {
                            //每次读文件流的2KB
                            dataRead = fs.Read(content, 0, BufferSize);
                            rs.Write(content, 0, dataRead);
                        } while (!(dataRead < BufferSize));
                        rs.Close();
                        MessageBox.Show("上传成功");
                    }
                        }
                catch (Exception ex) {
                    //上传过程出错，暂不处理 
                }
                finally
                {
                    fs.Close();
                }
 
            }
            ftp = null;
            // 可以记录一个日志  "上传" + fileinfo.FullName + "上传到" + "FTP://" + hostname + "/" + targetDir + "/" + fileinfo.Name + "成功." );     
        }
        /// <summary>
        /// 在ftp服务器上创建目录
        /// </summary>
        /// <param name="dirName">创建的目录名称</param>
        /// <param name="ftpHostIP">ftp地址</param>
        /// <param name="username">用户名</param>
        /// <param name="password">密码</param>
        public void MakeDir(string dirName, string ftpHostIP, string username, string password)
        {
            try
            {
                string uri = "ftp://" + ftpHostIP + "/" + dirName;
                System.Net.FtpWebRequest ftp = GetRequest(uri, username, password);
                ftp.Method = WebRequestMethods.Ftp.MakeDirectory;
 
                FtpWebResponse response = (FtpWebResponse)ftp.GetResponse();
                response.Close();
            }
            catch (Exception ex)
            {
             //   MessageBox.Show(ex.Message);
            }
        }
 
        /// <summary>
        /// 判断ftp服务器上该目录是否存在
        /// </summary>
        /// <param name="dirName"></param>
        /// <param name="ftpHostIP"></param>
        /// <param name="username"></param>
        /// <param name="password"></param>
        /// <returns></returns>
        private bool ftpIsExistsFile(string dirName, string ftpHostIP, string username, string password)
        {
            bool flag = true;
            try
            {
                string uri = "ftp://" + ftpHostIP + "/" + dirName;
                System.Net.FtpWebRequest ftp = GetRequest(uri, username, password);
                ftp.Method = WebRequestMethods.Ftp.ListDirectory;
 
                FtpWebResponse response = (FtpWebResponse)ftp.GetResponse();
                response.Close();
            }
            catch (Exception)
            {
                //ListDirectory方法，目录为空或目录不存在是报异常
                flag = false;
            }
            return flag;
        }
        private static FtpWebRequest GetRequest(string URI, string username, string password)
        {
            //根据服务器信息FtpWebRequest创建类的对象
            FtpWebRequest result = (FtpWebRequest)FtpWebRequest.Create(URI);
            //提供身份验证信息
            result.Credentials = new System.Net.NetworkCredential(username, password);
            //设置请求完成之后是否保持到FTP服务器的控制连接，默认值为true
            result.KeepAlive = false;
            return result;
        }
        #endregion
{% endhighlight%}


转载自:http://www.mcalvin.net/article.aspx?id=30