+++
date = "2017-01-04T04:51:53Z"
title = "Google App Engine: bad design for leaver"
slug = "gae_bad_design_for_leaver"
+++

Google App Engine will make you crazy after you use it and want get rid of it.

I follow GAE tutorial to create a go application for testing. Everything goes well, application running, a hello world response is received. I try to delete the application I created and back to normal life.

GAE told me the only way to delete GAE default application is delete project including it.  What?

I create GAE application in project which used by all my virtual machines. So it can’t be deleted. Anyway, what I want is just don’t let me receive billing about GAE application. So I try to stop the application. I visit `Versions` page and try to stop it. But the stop button remains disable. Why?

    You can only stop versions that are manually scaled, basic scaled, or in the flexible environment.

Although I’m not sure what does it mean, I guess I created the `hello world` application with some mode which can’t be stopped.

So I search google and use `app.yaml` to change my application to manual scaled application and deploy it.

    service: my-service
    runtime: nodejs
    env: flex
    manual_scaling:
      instances: 1

Now I can stop the version. Although I can’t delete all the versions, GAE doesn’t allow you to delete all versions when you created one.

So after several hours work, the thing seems to be done. And when I visit `Settings` page, there’s a `Disable application`, so I read the help text for it.

    Disable application

    Disabling an application will stop all serving requests, but you will not lose any data or state. Billing charges will still incur when applicable. You can re-enable your application at any time.

Billing charges will still incur when applicable? What does it mean? So please tell me if it charges after I disabled all the thing, OK?

Anyway, I will not try to use that product. Confused management， too many concept which is not meaning friendly for new users.
