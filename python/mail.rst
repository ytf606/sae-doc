.. include:: ../services/mail.rst

API使用手册
============

..  module:: sae.mail

..  py:class:: EmailMessage(**kwargs)
    :module: sae.mail

    EmailMessage类

    参数同下面的initialize

    ..  py:method:: initialize(\**kwargs)

        初始化邮件的内容。

        to: 收件人，收件人邮件地址或者收件人邮件地址的列表。

        subject: 邮件的标题。

        body/html: 邮件正文。如果内容为纯文本，使用body，如果是html则使用html。

        smtp: smtp服务器的信息。是一个包含5个元素的tuple。
        (smtp主机，smtp端口， 邮件地址或用户名，密码，是否启用TLS）。

        from_addr: 可选。发件人，邮件的from字段，默认使用smtp的配置信息。

        attachments: 可选。邮件的附件，必须为一个list，list里每个元素为一个
        tuple，tuple的第一个元素为文件名，第二个元素为文件的内容。

    ..  py:method:: send

        提交邮件发送请求至后端服务器。

    ..  py:method:: __setattr__(attr, value)

        attr: 属性名。 value: 属性的值。

..  py:function:: send_mail(to, subject, body, smtp, **kwargs)
    :module: sae.mail

    快速发送邮件。

    字段的意义同EmailMessage.initialize()。
    

使用示例
===============

1.  快速发送一份邮件 ::

        from sae.mail import send_mail

        send_mail("katherine@vampire.com", "invite", "to tonight's party",
                  ("smtp.vampire.com", 25, "damon@vampire.com", "password", False))

2.  发送邮件给多个收件人 ::

        to = ["katherine@vampire.com", 'rebecca@vampire.com', 'elena@vampire.com']
        send_mail(to, "invite", "to tonight's party",
                  ("smtp.vampire.com", 25, "damon@vampire.com", "password", False))
    

2.  发送一封html格式的邮件 ::

        from sae.mail import EmailMessage

        m = EmailMessage()
        m.to = 'damon@vampire.com'
        m.subject = 'Re: inivte'
        m.html = '<b>my pleause!</b>'
        m.smtp = ('smtp.vampire.com', 25, 'katherine@vampire.com', 'password', False)
        m.send()

3.  使用Gmail SMTP  ::

        import sae.mail

        sae.mail.send_mail(to, subject, body,
                ('smtp.gmail.com', 587, from, passwd, True))
