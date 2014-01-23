.. include:: ../services/cron.rst

使用示例
============

每五分钟请求一次 /backend/cron/update URL

Flask URL 处理程序 ::

    import datetime
    import pylibmc as memcache

    from appstack import app

    mc = memcache.Client(['localhost'])

    @app.route('/backend/cron/update', methods=['GET', 'POST'])
    def update():
        update_time = mc.get('update_time')
        mc.set("update_time", str(datetime.datetime.now()))

        return update_time

config.yaml ::

    name: appstack
    version: 4

    cron:
    - url: /backend/cron/update
      schedule: "*/5 * * * *"
