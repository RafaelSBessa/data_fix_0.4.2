
# Announcement: Bug in open ephys GUI that could have affected saved data

We just became aware of a bug in the GUI that could have affected the timestamps on saved data in some cases. The bug has now been fixed in the just released version 0.4.2.1, and if you are affected you can update your software now. We also posted methods and a script that can be used to correct existing data that has been affected on https://github.com/open-ephys/data_fix_0.4.2

(this announcement& bugfix relate to issue #154)

__Who is affected:__
This bug affects only the .continuous open ephys binary format, only when non-consecutive subsets of channels are saved using the channel selection panel of a processor in version 0.4.2 or earlier. For cases where the channel remapper is used to throw away unused data, and consecutive channels are saved sequentially later in the processing chain, there should not be an issue. Also, when using the "Use separate files" record option (accessible via the "R" button next to the record format selector), which creates different files for each recording, the bug does not apply.

__What kind of data corruption is expected:__
The bug manifests in two ways. 1 - There is occasional jitter in the saved time stamps, manifesting as gaps and jumps in the data. 2 - In some cases, data records might get corrupted in a way that leads to a "found corrupted record" error, with the load_open_ephys_faster matlab function.

![Example of affected data](https://raw.githubusercontent.com/open-ephys/data_fix_0.4.2/master/example_issue_data.png)

__How to test if you're affected:__
Even if you don't get a "found corrupted record" error, your data might be affected. Plot the derivative of the timestamps, and check if there are jumps. If you're in the affected group, but dont see errors, we recommend updating the GUI just in case.

![Example of affected timestamps](https://raw.githubusercontent.com/open-ephys/data_fix_0.4.2/master/example_issue_timestamps.png)

__How to handle existing corrupted data:__
For the corrupted record issue, we posted a script in https://github.com/open-ephys/data_fix_0.4.2 that should be able to correct data files by fixing the invalid record sizes. This script only corrects the structure of the data format, and does not affect the possibly affected timestamps.

For the time stamp jitter issue, the simplest fix is to disregard the affected timestamps, and to just make new ones from the sample rate and sample numbers. Alternatively, if testing the timestamps reveals a channel with timetsmps that are not affected, using these timestamps for all channels will also work. 

For cases where multiple recordings are combined into single files, the situation is somewhat more complicated because the timestamps in the first record of the sub-recordings could be jittered. In these cases, the messages.events file can be used to find the time that each recordings starts:

0 Software time: 145610@1000000Hz
0 Processor: 101 start time: 0@30000Hz

We recommend that you check your data and make sure that your analysis is not affected. If you have questions, do get in touch via our message board (open-ephys@googlegroups.com) or the GitHub [Issues](https://github.com/open-ephys/plugin-GUI/issues) page.










