# r-stringlines-nyc-mta-gtfs-train-visualization

# 🚀 History of Stringlines, how to read them, how to create them using R and GTFS for Transit Bus and Train visualizations 🚀

https://github.com/coding-to-music/r-stringlines-nyc-mta-gtfs-train-visualization

From / By UDAY SCHULTZ https://homesignalblog.wordpress.com/2022/11/26/stringlines/

https://twitter.com/A320Lga

NOVEMBER 26, 2022 ~ UDAY SCHULTZ

https://github.com/lgaa320/stringlines

### Helpful links for installing R

https://intro2r.com/install_r.html

https://www.dataquest.io/blog/installing-r-on-your-computer/

## Environment variables:

```java

```

## user interfaces:

## GitHub

```java
git init
git add .
git remote remove origin
git commit -m "first commit"
git branch -M main
git remote add origin git@github.com:coding-to-music/r-stringlines-nyc-mta-gtfs-train-visualization.git
git push -u origin main
```

# Stringlines!

From / By UDAY SCHULTZ https://homesignalblog.wordpress.com/2022/11/26/stringlines/

NOVEMBER 26, 2022 ~ UDAY SCHULTZ

https://github.com/lgaa320/stringlines

![stringline_S-Bahn-Berlin-GmbH-S7](/images/stringline_S-Bahn-Berlin-GmbH-S7.png)

If you follow [my Twitter account](https://twitter.com/A320Lga) or have read other articles on my blog, you will have likely seen a chart that looks like the above. These are what are called stringline (or Marey, or time-distance) charts, a very common tool used in transit and rail planning. Usually displayed with distance on the vertical axis and time on the horizontal, each one of the lines on these charts represents an individual vehicle moving along a line. This sort of visualization is incredibly useful for understanding the function of transport networks. By synthesizing the flow of traffic across a whole route, they allow dispatchers to monitor their territory, and help planners write timetables and work plans that minimize conflicts between vehicles. But stringlines can be much more than just a planning tool: they can provide a basis for interrogating the values, constraints, and risks within our transit networks. In this post, I hope to both illustrate these ways of reading stringlines, and also (for those interested) give instructions on how to make your own stringlines using widely available transit data.

## Origins

Stringline charts are a product of the managerial revolutions of the nineteenth century. [Sometime during the 1840s](https://sandrarendgen.wordpress.com/2019/03/15/data-trails-from-paris-with-love/), French railway schedulers began using them to write timetables on the increasingly busy bits of railroad connecting Paris with other major French cities (for a more detailed account, one from which this paragraph draws extensively, see here). It is essential for the smooth flow of traffic that trains moving in opposing directions or at different speeds meet at locations with overtaking or passing tracks, and while it is possible to design timetables to achieve that without the use of graphical aids, plotting trains on a chart and then aligning their schedules to intersect at points with more than one track makes that process quicker. This is exactly what French railway schedulers did, drawing trains as lines (with each lines’ slope corresponding to the trains imagined average speed) on their charts and constructing their route timetable upwards from that graphical, and geographical, framework.

![gallica.bnf.fr - Bibliothèque nationale de France](/images/stringline-1852-ligne_de_paris_a_boulogne.jpeg)

Source: gallica.bnf.fr / Bibliothèque nationale de France, found via [this blog post](https://sandrarendgen.wordpress.com/2019/03/15/data-trails-from-paris-with-love/) on Marey Charts’ history.

Over the ensuing century and a half, graphical timetabling proliferated across the world, and has become a mainstay of both scheduling and operations management. Even in this era of computer-aided dispatching and timetable planning, these visual representations remain a powerful tool for succinctly understanding interactions between vehicles and constraints across lines. The usefulness of these charts for planners is one that mirrors. Plotting a transit service’s timetable with a stringline allows an interested observer to understand much, much more about the operation of a given bit of service than can be ever gleaned from a tabular timetable.

## How to Read Stringlines, In Detail

Essentially all stringlines you will ever encounter work on the same basic principle: plotting distance on one axis, time on the other, and the paths of vehicles in between. The code I wrote — and which I will describe how to use below — is capable of implementing complex transformations of schedule data, so I want to offer a primer on how to read its outputs.

![B62 bus service between Long Island City and Downtown Brooklyn](/images/stringline_b62.png)

Take this stringline as an example. It shows B62 bus service between Long Island City and Downtown Brooklyn. Complicated, right? Let’s break down what’s going on.

![from the B62’s timetable](/images/b62-route.png)

Map [from the B62’s timetable](https://new.mta.info/document/7136)

The B62’s route is typical of an urban surface transit route in the Americas. It’s a mix of two way streets and paired running on one-way segments. The code reflects this by breaking up paired one-way running segments into separate sections in the plot, and showing the two-way sections as one. To show continuity of trips over sections of the plot devoted to buses travelling in the opposite direction, it uses thin lines; to show sections where buses in that direction are making stops, it uses thick lines, with dots to show where buses stop (big dots for labeled stops; little dots for non-labeled).

![combined N and W timetable](/images/stringline_mta-N-and-W-Broadway-Local.png)

_For clarity, not all stops are shown._

If you’re plotting two routes simultaneously, the same principle applies. Here’s a plot showing the combined N and W timetable. The two routes run together from Astoria to 42nd Street, where the N becomes an express to Brooklyn (continuing to share platforms with Ws until 14th St), and the W stays local to Whitehall Street. The thin lines help show N service’s continuity over the W-only part of the plot.

You can make things pretty complicated with this code! It will, for example, allow you to plot multiple bus lines with separate-direction segments, odd branching on commuter rail networks, and so on. But these basic principles of how to read the plots are pretty constant. More on how to make these charts later.

## Budget-Driven Headways: MBTA’s 94 Bus

As a transit-oriented student at Harvard, I have always been fascinated by the odd headways used on some of the buses in the MBTA network. A good example is the 61, which run from Waltham into some office parks along Route 128. Its Saturday service runs on a 50 minute headway, putting easy-to-remember clockface timetables out of riders’ reach.

![Alt Text](/images/XXXXXXX)

Plotting a stringline of the route revealed why. The 61’s timetable runs with just one bus, shuttling back and forth between Waltham and the office parks. With one-way running times of about 20 minutes and a good bit of time for layovers at each end, the route ends up with a 50 minute headway (and a 60 on weekdays, when there’s more traffic).

## Creating Risk on the Weekend F Train

Timetables tell stories. Those of the weekend F and G trains reveal service planning’s intersection with repeated service failures on the New York City subway. The F and G trains, for those not familiar, are the two subway services which run on Brooklyn’s Culver Line. The F runs to Stillwell Avenue (with some weekday peak trains turning at Kings Highway), but thanks to the relatively low ridership of F stops below Church Avenue, the G turns there.

![Alt Text](/images/XXXXXXX)

_Note how every 5th F train’s schedule gets shifted by a few minutes at Kings Highway, towards the bottom of the plot, to accommodate G trains entering service at Church._

As anyone who rides the lower part of the F can tell you, the G’s terminal at Church Avenue [causes problems](https://twitter.com/A320Lga/status/1582107745997574144?s=20&t=dqcfMPG8QcZv7ZcLihJa2Q). Southbound G trains often hold up Fs as they’re cleared of passengers before heading into tracks beyond Church Avenue where they turn around. Perhaps more surprisingly, northbound Gs regularly cause delays in F service as they enter service at Church Avenue. One might think that G trains entering service on schedule, and F trains still relatively near their origin terminals would make for a problem-free interaction at Church Avenue’s northbound platform — especially on weekends, when frequencies are lower. The reason that’s not the case has everything to do with timetables.

[Back in the day](https://homesignalblog.wordpress.com/2021/08/15/maintenance-process-and-the-future-of-the-off-peak-subway/) (i.e. in 2006) both F and G trains ran every eight minutes. As maintenance policy-driven weekend capacity reductions have taken their toll on the system’s function, service levels have fallen. So today, the F, hard-hit by construction impacts, runs every 12 minutes, and the G runs every 10. This asymmetric headway is at the heart of the Church Avenue problem, and is readily visible on a stringline. Running both these services at their average headways would lead to conflicts (eg. Fs at the :00, :12, :24, :36, :48 and Gs on :06, :16, :26, :36, :46, :56 would conflict at :36 past every hour), so planners shift conflicting Fs slightly later to accommodate Gs. On paper, the timetable ‘works’ but this solution is fragile. Even the slightest perturbation in F or G service can lead to conflicts and delays at Church Avenue — which is exactly what happens a dozen or so times a day, every weekend (and weekday; F/G frequencies aren’t matched then either).

## Transit’s Priorities: The Pascack Valley Line

New Jersey Transit’s Pascack Valley Line is one of New York’s lesser known corridors. It has neither the ridership of the Long Island Rail Road’s Babylon Branch, the scenery of Metro North’s Hudson Line, or the complexity of NJT Morris & Essex Line. Its timetable is one of the clearest encapsulations of commuter rail’s bent towards peak service — and the political factors which keep it that way.

![Alt Text](/images/XXXXXXX)

The first thing you might notice about this stringline is its directional structure. In the morning all trains run to Hoboken; in the afternoon, you have a mix; and in the evening, all run from Hoboken. If you spend thirty seconds looking at a map of New Jersey, it’s not hard to tell why: Hoboken (and Secaucus, one stop to its north) are where riders can connect to trains and ferries into Manhattan’s Central Business District. This timetable is peak, white-collar oriented railroading in the flesh; the thousands traveling from Jersey City and Hoboken to jobs, friends, stores and appointments in Hackensack and beyond simply cannot use the line for their trips.

If you sit with this chart for longer, a deeper problem becomes clear. Especially when looking at railroad timetables, something always worth noting is where trains pass each other. On this stringline, those ‘meets’ take place in three places: between Pearl River and Nanuet, between Anderson Street and New Bridge Landing, and between Teterboro and Hoboken. That’s not an accident. While the Pascack was once [double tracked as far as Oradell](https://www.google.com/books/edition/Railroad_Gazette/nCA2AQAAMAAJ?hl=en&gbpv=1&dq=oradell+%22double+track%22&pg=PA570&printsec=frontcover), it today is a single track line from Teterboro to Spring Valley, with passing sidings at Anderson Street and Pearl River. The fact that it takes a bit more than half an hour for a local train to travel between those two sidings is what transforms heavy, core-oriented peak commuting volumes into the absence of service for other types of travel. It is simply impossible to run the high-density peak direction service that decisionmakers want and run meaningful reverse-peak service. The off-peak portion of the stringline makes it easy to understand why. If an inbound and outbound train meet at New Bridge Landing, half an hour will elapse as the outbound train makes its way up to Nanuet and meets the next inbound. In turn, another half hour will go by as the inbound train makes its way down to New Bridge — add it together, and you have a minimum headway of an hour or more.

Here’s the real kicker: New Jersey Transit tried to fix this. In 2003, the agency proposed adding several new passing sidings to the line to break up that distance and allow for reverse-peak service. One would think that the towns which would get much-improved train service from this new infrastructure would welcome it — but in reality, a coalition of six towns ended up suing New Jersey Transit alleging (just about baselessly) that the sidings were actually a ploy to introduce more freight service on the route. These NIMBYs won; only [four of the planned six](https://www.recordonline.com/story/business/2007/10/29/pascack-riders-get-more-trains/52714410007/) new sidings were built. The long gap survived efforts to remove it, and with it, poor service levels for non-peak travelers. A stringline cannot give you all this historical context, but in showing how service works it can help reveal salient operational problems and structures on a line, and provide a starting point for questioning the political histories of transit infrastructure.

## What These Charts Hide

Stringlines are a powerful visualization tool, but they cannot tell you everything about a transit route. Like every chart, they are abstractions of reality. Some of those are visible on stringlines, but are unexplained; those are ones you can piece apart with “why” questions of the sort I explored above. Others are less obvious, and to be an erudite reader of strings, you must be attentive to things they fundamentally cannot display.

When making stringlines with public agency schedule data, you are necessarily getting a limited version of reality. Trains and buses moving along a line don’t accelerate instantaneously and travel at constant speeds between stops. The motion of vehicles between two stops shown on a string chart is complex, and its complexity only increases as stops get further apart. So, for example, if you’re looking at a chart of the Metro North New Haven Line, you should keep in mind that the express trains that make no stops between 125 St and Stamford move in ways poorly reflected in the straight, constant-speed line that connects them on the plot.

![Alt Text](/images/XXXXXXX)

Similarly, transit vehicles are not point masses. They take up physical space, and in railroad contexts, they occupy signal space as well. To give an extreme example, a stringline will represent the progression of an 8,000 foot freight train on a route with signals every two miles as a line like any other, but in reality, the train not only occupies 8,000 feet of space behind its line, but also causes signals stretching back 6 miles from its rear end to display worse-than-green aspects. If you have data on signal systems and train lengths, you can find ways of representing those realities — but public GTFS schedule feeds do not allow that.

Lastly, it’s always important to remember that transit services are networked. Stringlines can help you understand the movement of vehicles on a route, but oftentimes, there are inter-route relationships that play a role in determining schedules. Planners might have optimized service on a bus route to make timed connections with a perpendicular route at an important intersection, or timed train service to simplify interline crewing. More so than issues of variable movement or signaling, these interconnections are understandable with research. However, that research must extend beyond the confines of just one line — though they are tools for interpreting one route, stringlines must be given network context.

## Making Your Own

Now, the fun part: making your own stringlines! I have written a code that should allow you to plot most agencies’ schedule data as string plots. Here’s how to use it. If this is hard to follow, I have configured the code to make a plot of 2 and 3 train service in New York, so you can start by running that and seeing how these steps impact it.

Step 1: Install R and RStudio (both free).

A good tutorial on how to do this can be found [here](https://www.dataquest.io/blog/installing-r-on-your-computer/); if you want to familiarize yourself with the basics of R before beginning to use the code, try this [tutorial](https://intro2r.com/).

Step 2: Download my code from [here](https://github.com/lgaa320/stringlines/blob/main/strings_consolidated_final3.R).

Step 3: Open the code in RStudio

Step 4: Install all the packages listed at the top of the code in the libraries() section.

R may prompt you to do this automatically; if not, type install.packages(“[name of package]”, “[name of next package]”) and so on in the command line, or use the packages tab on the right of the RStudio screen.

Step 5: Find your agency’s GTFS feed.

[Transit.land](https://www.transit.land/) and [transitfeeds.com](https://transitfeeds.com/) are my go-tos for schedule data, but if you can’t find what you want there you might also try searching [agency name] GTFS in your search engine — it has worked for me in the past.

Step 6: Download the GTFS feed to your computer.

Step 7: Check to see whether your GTFS is suitable for this script.

Paste the path to the GTFS feed into the box on line 29 of the code. Then, select lines 1 through 69 and either click run or do command (mac)/control (PC) + enter. You can also paste the link to the download URL of a GTFS feed, as I have done in my example.

The only requirement of this code is that the GTFS must have a stop_sequence column in the stop_times object. Additionally, the route you are plotting must not contain internal loops or reversals.

You can figure these things out by typing View(dat$stop_times) into the command line of your RStudio window and by looking at a (geographical) route map of your line. But if you aren’t sure about any of these points, or are just lazy, I would recommend just running the code. Your computer won’t explode; you’ll just see a messy plot and/or a bunch of error messages.

Step 8: Figure out the date range of your GTFS.

If you’re downloading data from transit.land or transitfeeds, the download pages for the feed will tell you its start/end dates; if not, type View(dat$.$dates_services) into the command line and see the dates included in the feed. Choose an included date, and alter the date_tar argument to reflect that choice.

Step 9: Choose your routes.

Some GTFS feeds have intuitive route_ids (eg. the A train in NYC is “A” in the subway’s GTFS), and others do not. You can find route_ids in two ways. Within the code, you can type View(dat$routes) into the command line and find your route (usually the route_short_name and route_long_name fields have the common names for the services). You can also look the IDs up on transitfeeds or transit.land.

Note: some routes will have multiple route_ids associated with them. Unless there’s a clear difference between them (eg. one corresponds to the night rail replacement shuttle, and the other rail service, or one is northbound service and the other is southbound), I would suggest putting one ID in the routes_tar field (line 37) and seeing what it produces, perhaps with the other route_ids in the routes_secondary field (line 45).

If all of this seems a bit confusing, I have built a workaround into the code. If, when you type View(dat$routes) into R, you see a route_short_name column with values in it, you can use those values instead of dealing with route_ids. Simply change line 39 to TRUE, and you’re on your way. This was quite useful for me, to give one example, when trying to plot Berlin’s S7, which has 4 route_ids associated with it — it saved me having to figure out which one(s) were actually in use.

When entering routes into the routes_tar and routes_secondary fields, there are some important rules/distinctions you should know.

The routes_tar field can accept up to two routes that:

Overlap for at least two stops
Have the same direction_ids at those stops
The code will plot the entirety of all routes in the routes_tar field (as shown in the N and W plot above, and the 2/3 example plot). If you’re not sure whether a combination will work, try it. This functionality can get messy so perfect results are not guaranteed, but it works for me about 90% of the time.

The routes_secondary field can take pretty much anything. It will plot the portions of the routes you put into it that overlap with the route(s) you entered in the routes_tar field. If direction_ids don’t match, things can get wonky — but if you plot both directions, problems usually disappear.

Step 10: Choose your directions.

Because the 1 and 0 values don’t always correspond intuitively with, well, anything, I would try running the whole code before choosing a direction to plot. Moreover, some GTFSes (eg. Trenord) have both directions of train service coded as one. The iterative method here can really save you some pain.

Step 11: Choose methodological factors.

Template_choice: if you want to view stringlines based on the most common route variant, use “Most” here; if you want to see them based on the longest, use “Longest.”
Mand_stop: if, when you plot your stringline, the code has chosen a route variant that does not include some branch or stop of interest to you, look up a stop on that branch. To make your life easier, run 1-245, then paste View(stop_times%>%inner_join(.,trips%>%select(trip_id,direction_id,route_id)%>%inner_join(.,stops%>%select(stop_id,stop_name)) into the command line. Find your stop of interest, and paste its stop_id(s) into this argument to force the code to choose variants which serve it. Be sure to include stop_ids for each direction of service!
Name_elim: this is a purely aesthetic argument that removes some stops from the final plot to make the y axes less cluttered.
Time_start and time_end. These ones are pretty self-explanatory, but it’s probably worth a reminder that you need to give start/end times which will encompass some amount of service on the line you’re plotting. Asking this script to plot, say, overnight train service in Washington DC will lead to meaningless and error-ridden results. Trust me, I have made that mistake more times than I care to admit.
Step 12: Run it!

If you have already downloaded your GTFS, select lines 30-1039 and do command or control + enter. Once the plot has been made, you can save it (which often makes it more legible, because you can adjust the size of the plot image output) using the final two lines of the script.

Step 13: Troubleshooting

This code isn’t perfect, in part because I have zero (0) formal training in R, and in part because transit systems and their data feeds are legitimately complicated. When things don’t work, I usually try:

Ensuring I haven’t made some configuration error (eg. choosing an invalid date, or put route_ids into the routes fields while leaving the use_rt_sht set to TRUE).
Changing the template_choice method to get a different shape
Changing route_ids, or changing to route_short_name filtering
Looking at a map of the route to ensure it doesn’t have any internal loops or reversals
I am certain there are parts of this that will break even with these fixes, or potential errors I did not catch. My email is in the “about” page of this blog. No guarantees I’ll get to more complex problems quickly (I’m a 21 year old thesis-writing and job-hunting college senior, at the end of the day), but I really genuinely do want this to be as useful as a tool as possible for transit advocates/enthusiasts so do shoot me a message if needed. And of course, this code is a work in progress; check back here for updates.

Happy plotting!
