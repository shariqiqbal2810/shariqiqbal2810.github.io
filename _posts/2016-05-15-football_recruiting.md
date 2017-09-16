---
layout: post
title:  "Is There A Geographic Bias in Football Recruiting Rankings?"
date:   2016-05-15 14:00:00
categories: [sports]
tags: [datavis, football, python]
---
In the modern era of college football, teams from the Southeast United States have had a remarkable run of success. Since 2003, teams from the SEC have won 9 national championships. Many attribute this success to the higher quantity  of highly ranked high school players coming out of the region, but, recently, I began to wonder if the ranking of high school recruits has become biased due to the dominance of college teams from the region. I thought it would be interesting to analyze some geographic player recruiting data and compare to their NFL draft performance in the interest of exploring these hypotheses. The full IPython Notebook I used for this analysis with code and plots can be viewed [here](https://github.com/shariqiqbal2810/sports_analysis/blob/master/regional_football_recuiting_comparison.ipynb).

I started by scraping the 24/7 sports football composite recruit rankings (which aggregates all of the major recruiting services' rankings into one definitive list) using an html parsing library for Python called lxml. It allows for really simple, yet powerful, commands that can pull structured data from a web page. For example, the following code downloads a page from the 24/7 site, and pulls the names of all of the recruits on the page.

{% highlight python %}
import requests
from lxml import html

page = requests.get('http://247sports.com/Season/2000-Football/CompositeRecruitRankings')
tree = html.fromstring(page.content)
names = tree.xpath('//a[@class="bold"]/text()')
{% endhighlight %}

The key here is the last line that calls `xpath`. This command searches through html tags. In this case, it's searching for `a` tags (which are links in html) that contain the element, `class` set to the value `bold`. It then returns the text contained in all of the specified tags in a list. A few commands like this combined with a quick look at the source code for a page is enough to pull all of the data into a nice workable data frame. In this case, these are the types of tags that surround the names of each recruit. In addition to names, I also grabbed rankings and high school location.

After pulling the information for every top 50 recruit from 2000-2010, I proceeded to scrape Wikipedia's list of NFL draft selections from 2003-2015 (to account for early draft entries and 5th-year seniors) and matched up recruit names in the lists of drafted players in order to record the position each player had been picked, if they had been picked at all. I'm using NFL Draft position as a metric for performance in college since there is no statistical metric for player performance across positions (like a more general QBR) and it should, ideally, be an independent evaluation from their high school ranking. Additionally, many recruiting services state that their goal is to predict NFL potential with their rankings, so it seems like the most like-for-like comparison.

It has been well established that the U.S. Southeast produces the highest volume of elite recruits per capita, but it produces some interesting visualisations nonetheless.

<img src="/images/recruiting_analysis/recruit_density_barplot.png">

<img src="/images/recruiting_analysis/recruit_density_map.svg">

Unsurprisingly, the southeast is the clear winner in terms of elite recruits per capita. A few questions remain, though: Do recruiting ratings accurately reflect NFL draft performance? Is there a geographic bias in the rankings towards players from recruiting hotbeds? And finally, are players from the southeast actually outperforming their rankings (leading to better success for these programs at the college level)? In order to move toward answering these questions, I plotted recruiting ranking vs NFL draft performance (proportion drafted and draft position for those who were drafted) and separated by region.

<img src="/images/recruiting_analysis/prop_drafted.png">

<img src="/images/recruiting_analysis/draft_pick.png">

It's hard to draw conclusions since there are many factors at play here, but the general trend seems to be that recruiting rankings do predict success to a certain extent; however, the variability of draft performance increases as the rankings get lower. No region seems to clearly dominate or lag behind across all rankings, suggesting that recruiting analysts do not exhibit a geographical bias when ranking players. They specifically do not appear to be underrating or overrating players from the Southeast. It seems reasonable to conclude, then, that the success of SEC teams is likely bolstered by the amount of highly ranked recruits the region produces, and these rankings actually do a good job of remaining unbiased, despite vast gaps in quality of high school play across regions.

