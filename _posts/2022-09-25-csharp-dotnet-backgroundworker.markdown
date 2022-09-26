---
layout: post
title:  C# .Net BackgroundWorker使用
date:   2022-09-25 19:46:02
---

多线程BackgroundWorker使用

```
void Button1Click(object sender, EventArgs e)
{
	using (BackgroundWorker bw = new BackgroundWorker())
	{
		bw.RunWorkerCompleted += new RunWorkerCompletedEventHandler(bw_RunWorkerComplete);
		bw.DoWork += new DoWorkEventHandler(bw_DoWork);
		bw.RunWorkerAsync("Tank");
	}
}
		
void bw_DoWork(object sender, DoWorkEventArgs e)
{
	Thread.Sleep(3000);
	e.Result = e.Argument + "工作线程完成";
}
		
void bw_RunWorkerComplete(object sender, RunWorkerCompletedEventArgs e)
{
	this.label1.Text = e.Result.ToString();
}
```