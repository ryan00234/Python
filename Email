# -*- coding: utf-8 -*-
# Created by rengl on 2018/3/6
from imapclient import IMAPClient
from bs4 import BeautifulSoup


class Imapmail(object):

    def __init__(self):  # 初始化数据
        self.serveraddress = None
        self.user = None
        self.passwd = None
        self.prot = None
        self.ssl = None
        self.timeout = None
        self.savepath = None
        self.server = None

    def client(self):  # 链接
        try:
            self.server = IMAPClient(self.serveraddress, self.prot, self.ssl, timeout=self.timeout)
            return self.server
        except BaseException as e:
            return "ERROR: >>> " + str(e)

    def login(self):  # 认证
        try:
            self.server.login(self.user, self.passwd)
        except BaseException as e:
            return "ERROR: >>> " + str(e)

    def getmaildir(self):  # 获取目录列表 [((), b'/', 'INBOX'), ((b'\\Drafts',), b'/', '草稿箱'),]
        dirlist = self.server.list_folders()
        return dirlist

    def getallmail(self):  # 收取所有邮件
        gitaddr = []
        print(self.server)
        self.server.select_folder('INBOX', readonly=True)  # 选择目录 readonly=True 只读,不修改,这里只选择了 收件箱
        result = self.server.search()  # 获取所有邮件总数目 [1,2,3,....]
        print("邮件列表:", result)
        for _sm in result:
            # data = self.server.fetch(_sm, ['ENVELOPE'])
            # size = self.server.fetch(_sm, ['RFC822.SIZE'])
            # print("大小", size)
            # envelope = data[_sm][b'ENVELOPE']
            # print(envelope)
            # subject = envelope.subject.decode()
            # if subject:
            #     subject, de = decode_header(subject)[0]
            #     subject = subject if not de else subject.decode(de)
            # dates = envelope.date
            # print("主题", subject)
            # print("时间", dates)

            msgdict = self.server.fetch(_sm, ['BODY[]'])  # 获取邮件内容
            mailbody = msgdict[_sm][b'BODY[]']  # 获取邮件内容
            soup = BeautifulSoup(mailbody)
            gitaddr.append(soup.find_all('td')[5].string)
        data = set(gitaddr)
        print(data)
        return data
        # for td in soup.find_all('td'):
        #     # gitaddr = re.match('git', str(td.string)).start()
        #     if td.string == None:
        #         pass
        #     else:
        #         print(td.string)
        # with open(self.savepath + str(_sm), 'wb') as f:  # 存放邮件内容
        #     f.write(mailbody)

    def close(self):
        self.server.close()


if __name__ == "__main__":
    imap = Imapmail()
    imap.serveraddress = "mail.google.com"  # 邮箱地址
    imap.user = "ryan00234@gmail.cn"  # 邮箱密码
    imap.passwd = "woshishui"  # 邮箱账号
    imap.savepath = "/Users/renguoliang/Desktop"  # 邮件存放路径
    imap.client()
    imap.login()
    data = imap.getallmail()
    imap.close()
