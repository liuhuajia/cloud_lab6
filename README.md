# cloud_lab6

## 增加prometheus Exporter
没有做很多修改，只是在原有的指标基础上新增了一个反应CPU当前利用率的指标，具体代码修改如下：     

**新增指标的定义**

		import (
			"github.com/shirou/gopsutil/cpu"
			"github.com/prometheus/client_golang/prometheus"
			"time"
		)
		var (
			…………
			usage = prometheus.NewGauge(
				prometheus.GaugeOpts{
					Name:      "cpu_usage",
					Help:      "system cpu usage.",})
		)
		
**注册新增指标**   

	func Register() {
		prometheus.MustRegister(requestCount)
		prometheus.MustRegister(requestLatency)
		prometheus.MustRegister(cpu)
	}
	
**赋值**

	func RequestIncrease() {
		requestCount.WithLabelValues().Add(1)
		cpu_usage,_ :=cpu.Percent(time.Second, false)
		usage.Set(cpu_usage[0])

	}
