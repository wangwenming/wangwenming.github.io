Tomcat 启动定时任务：
1. ServletContextListener
2. Spring的applicationContext.xml 配置 task:scheduler (轻量级的Quartz)
    <task:scheduler id="scheduler" pool-size="10" />
    <task:executor id="executor" pool-size="10" />
    <task:annotation-driven scheduler="scheduler" executor="executor" />


3. java.util.Timer / java.util.TimerTask
4. Quartz
5. dwlc-web项目使用的是：
import org.springframework.scheduling.annotation.Scheduled;


@Scheduled(cron = "0 0/25 * * * *")
com.voiinnov.dwlc.cache.CrontabTask.queryBindBank()

管理后台使用的是 quartz (/voiinnov-dwlc-manage/WebContent/WEB-INF/lib/quartz-all-1.8.6.jar)

com.brick.initJob.dueJob

src/config/sql/map/warningCenter/warningEntry.xml
<select id="selectEntryVo" parameterClass="map" resultClass="WarningEntry">


package com.brick.warningCenter.core.WarningEngine.java
import org.quartz.SchedulerException;

    }

    public synchronized Scheduler getScheduler() throws SchedulerException{
        if(scheduler == null){
            scheduler = new StdSchedulerFactory().getScheduler();
        }
        return scheduler;
    }