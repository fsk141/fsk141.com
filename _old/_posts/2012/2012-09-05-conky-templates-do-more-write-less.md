---
layout: post
title: "Conky Templates (Do more, Write Less)"
description: "Conky is a powerful tool that gives you information and statistics about your PC. With all of the possible variables that you can choose from, and multiple things you can monitor it becomes easy to have a bloated config. Repeated content, color associations, naming conventions, etc. Repetitive information such as network interfaces, cpu monitoring, and hdd info can easily be templated. Not only to make your config easier to read, but will save you a caboodle of keystrokes from not having to type repetitive content."
category: Linux
tags: [conky, conky templates]
---
{% include JB/setup %}

##Introduction:
Conky is a powerful tool that gives you information and statistics about your PC. With all of the possible variables that you can choose from, and multiple things you can monitor it becomes easy to have a bloated config. Repeated content, color associations, naming conventions, etc. Repetitive information such as network interfaces, cpu monitoring, and hdd info can easily be templated. Not only to make your config easier to read, but will save you a caboodle of keystrokes from not having to type repetitive content.

<a href="/a/2012-09-05-conky/conky_code.png">{% thumbnail a/2012-09-05-conky/conky_code.png 600x600> %}</a>

##Diving in:
Moving right along, I assume that you know the basics to conky. This isn't a primer for conky, rather just a primer on conky templates.	Conky templates are one of the more powerful features to conky, and I'm always suprised to see so many people using repetitive code in their ~/.conkyrc. I aim to convert you :)

The conky man page describes templates as such:

	templateN
	Define a template for later use inside TEXT  segments.  Substitute N  by  a digit between 0 and 9, inclusively. The value of the variable is being inserted into the stuff below TEXT at the corresponding  position,  but before some substitutions are applied:
	'\n' -> newline
	'\\' -> backslash
	'\ ' -> space
	'\N' -> template argument N

Simply put, you have the ability to create functions that you can call from within your ~/.conkyrc. To get started lets draft a sample template &amp;.

###Declaring the template:
Open your ~/.conkyrc with your favorite text editor, and start by declaring a template:
{% highlight bash %}
#template0 (cpu) 1:name 2:number
template0 ${color0}\1) ${color}${freq_g \2}Ghz | ${cpu \2}% ${alignr}${cpubar \2 5,70}
{% endhighlight %}

The following code is what I use for CPU monitoring in my .conkyrc. The first line is a comment defining the template:

	#template0 -- name of the template (must be in the form of templateN (where N is a number 1-10)
	(cpu) -- friendly name
	1:name -- name to display on conky output
	2:number -- number of cpu to pull stats from
	
To specify a new variable, you just need a backslash followed by a number. As you can see in my example I have \1 &amp; \2.

	template0 -- template number
	${color0}\1) -- name of cpu -- 1)
	${color} -- everything after this be std color (defined at top of config)
	${freq_g \2}Ghz | -- get frequency of cpu -- 1.60Ghz |
	${cpu \2}% -- get % cpu utilization -- 10%
	$alignr}${cpubar \2 5,70} -- show graph of current utilization

###Calling the template in your ~/.conkyrc
Now that you have the building blocks setup, let's call the template, and put it to use:
{% highlight bash %}
TEXT
${color0}UpTime: ${color}$uptime
${color0}Kernel: ${color}$kernel
 ${template0 1 cpu0}
 ${template0 2 cpu1}
 ${template0 3 cpu2}
 ${template0 4 cpu3}
{% endhighlight %}

This is the first few lines of my TEXT portion of ~/.conkyrc (the parsable section of conky) I'm displaying the uptime, kernel version, and then using the cpu template (template0) to display my processor stats. To call the template use the template name, and provide the variables. (first variable is name, second is cpu identifier)

You will end up with something like the following:

<a href="/a/2012-09-05-conky/conky_cpu-template.png">{% thumbnail a/2012-09-05-conky/conky_cpu-template.png 600x600> %}</a>

Imagine if you wanted to display the same thing without templates, as you can see, templates rock!:
{% highlight bash %}
${color0}1) ${color}${freq_g cpu0}Ghz | ${cpu cpu0}% ${alignr}${cpubar cpu0 5,70}
${color0}2) ${color}${freq_g cpu1}Ghz | ${cpu cpu1}% ${alignr}${cpubar cpu1 5,70}
${color0}3) ${color}${freq_g cpu2}Ghz | ${cpu cpu2}% ${alignr}${cpubar cpu2 5,70}
${color0}4) ${color}${freq_g cpu3}Ghz | ${cpu cpu3}% ${alignr}${cpubar cpu3 5,70}
{% endhighlight %}

I have included my current ~/.conkyrc for a few examples relating to templates (I template everything!)

{% highlight bash %}
background no
update_interval 2.0
total_run_times 0
own_window yes
own_window_transparent yes
own_window_type override
double_buffer yes
maximum_width 300 
draw_shades no
draw_outline yes
draw_borders no
stippled_borders 0
border_width 1
color0 ffffff
default_color d9d9d9
default_shade_color c80000
default_outline_color 383838
alignment middle_right
gap_x 25
gap_y 15
no_buffers yes
cpu_avg_samples 2
net_avg_samples 2
text_buffer_size 2048

#template0 (cpu) 1:name 2:number
template0 ${color0}\1) ${color}${freq_g \2}Ghz | ${cpu \2}% ${alignr}${cpubar \2 5,70}

#template1 (memory) 1:name 2:type
template1 ${color0}\1   ${color}${\2perc}%/${\2max} ${alignr}${\2bar 5,70}

#template2 (hdd) 1:name 2:mountpoint
template2 ${color0}\1   ${color}${fs_used \2}/${fs_size \2} ${alignr}${fs_bar 5,70 \2}

#template3 (network - wired) 1:name 2:interface
template3 ${if_up \2}${color0}\1 ${color}${addr \2}\n ${color0}Down: ${color}${downspeed \2} \n   ${color0}Total Down: ${color}${totaldown \2} ${alignr}${voffset -8}${downspeedgraph \2 10,70}\n\n  ${color0}Up: ${color}${upspeed \2}\n   ${color0}Total Up: ${color}${totalup \2} ${alignr}${voffset -8}${upspeedgraph \2 10,70}${endif}

#template4 (network - wireless) 1:name 2:interface
template4 ${if_up \2}${color0}\1 ${color}${addr \2}@${wireless_essid \2} | ${wireless_link_qual_perc \2}%\n  ${color0}Down: ${color}${downspeed \2} \n   ${color0}Total Down: ${color}${totaldown \2} ${alignr}${voffset -8}${downspeedgraph \2 10,70}\n\n  ${color0}Up: ${color}${upspeed \2}\n   ${color0}Total Up: ${color}${totalup \2} ${alignr}${voffset -8}${upspeedgraph \2 10,70}${endif}


TEXT
${color0}UpTime: ${color}$uptime
${color0}Kernel: ${color}$kernel
 ${template0 1 cpu0}
 ${template0 2 cpu1}
 ${template0 3 cpu2}
 ${template0 4 cpu3}

${color0}Memory:
 ${template1 Mem mem}
 ${template1 Swap swap}

${color0}Disks:
 ${template2 /root /}
 ${template2 /home /home}

${color0}Network:
 ${template3 eth0 eth0}${template3 usb0 usb0}${template4 wlan0 wlan0}

${color0}Tasks:
${execi 5 for i in `/usr/bin/todo.sh -n -d ~/Dropbox/Public/todo.txt/todo.cfg lsprj`;
do echo -e "\n$i:\n`/usr/bin/todo.sh -n -d ~/Dropbox/Public/todo.txt/todo.cfg ls | grep $i | sed 's/^[0-9][0-9]//'`"
done}
{% endhighlight %}

Which ends up looking like this:

<a href="/a/2012-09-05-conky/conky_config-display.png">{% thumbnail a/2012-09-05-conky/conky_config-display.png 600x600> %}</a>

##Conclusion
Templates have been around for quite some time, yet a lot of ~/.conkyrc's floating on the internet don't take advantage of templating power. Spend a few minutes and update your config, not only will your config become smaller, but will instantly gain attractiveness &amp; readibility.
