Task 3: dataset exploration (clean_dialog.csv)

How big is the dataset?
du -sh clean_dialog.csv -> 4.7M
wc -l clean_dialog.csv -> 36860 lines (1 header + 36859 rows)
csvstat --count clean_dialog.csv -> 36859 rows

What is the structure of the data?
head -n 1 clean_dialog.csv -> "title","writer","pony","dialog"
Each row is one spoken line: title is the episode, writer is the episode writer, pony is the speaker, dialog is the line of text.

How many episodes does it cover?
csvcut -c 1 clean_dialog.csv | sort -u | wc -l -> 198, which includes the header "title", so 197 episodes
(csvtool was not available on Amazon Linux 2023, so I used csvcut from csvkit, installed with pip install csvkit)

Unexpected aspect of the dataset
csvcut -c 3 clean_dialog.csv | sort | uniq -c | sort -rn
The pony field is not one clean speaker name per line. It has combined speakers (Rainbow Dash and Applejack, Twilight Sparkle and Spike), variants of the main ponies (Mean Twilight Sparkle, Young
Twilight Sparkle, Pinkie Pie 2), group labels (All, Others, Main cast) and typos (Trxie, Mrs Cake vs Mrs. Cake). Counting a pony's lines depends on whether these count, so I only counted lines where
the pony field is exactly the pony's name.

Task 4: speaker frequency
grep -c ',"Twilight Sparkle",' clean_dialog.csv -> 4745
grep -c ',"Rarity",' clean_dialog.csv -> 2660
grep -c ',"Pinkie Pie",' clean_dialog.csv -> 2833
grep -c ',"Rainbow Dash",' clean_dialog.csv -> 3072
grep -c ',"Fluttershy",' clean_dialog.csv -> 2109
Percent of all lines = count * 100 / 36859, computed with awk, for example:
awk 'BEGIN { printf "Rarity,2660,%.2f\n", 2660 * 100 / 36859 }' >> Line_percentages.csv

