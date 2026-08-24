# minor_project_1
this project is mainly about the  DNA of the whatsapp chat  and it easily explain the complete funtions of the whatsapp chat DNA and that also provides the important facts 
# GroupDNA — WhatsApp Group Chat Analyzer

GroupDNA turns an exported WhatsApp group chat into a set of conversation insights using pure Python. It is designed to run as a Google Colab notebook and produces a shareable SVG dashboard plus a JSON summary of the analysis.

> **Privacy note:** WhatsApp exports can contain personal information. Review the generated dashboard and JSON before sharing them.

## What it analyzes

The notebook extracts messages into records containing a date, time, sender, and message text. It then reports:

- Total messages and unique group members
- Message-type counts: text, media, links, and deleted messages
- Ranked contributors and each member's activity percentage
- Hourly activity and the peak/busiest hour
- Most active member
- Most-used words, after applying the notebook's built-in English stop-word list
- Most-used emojis and the member with the highest emoji count ("Emoji King")
- Longest message and average message length
- Conversation starters, defined as the first sender on each new date
- Average response times for consecutive messages from different senders, excluding gaps longer than 24 hours
- Busiest weekday, active-date count, automatic insights, and a custom GroupDNA engagement score

## Outputs

Running the notebook creates and downloads these files:

| File | Description |
| --- | --- |
| `GroupDNA_Dashboard.svg` | A 1200 × 1800 SVG poster with KPI cards, contributor and message-type charts, activity highlights, top words, emojis, insights, and the GroupDNA score. |
| `GroupDNA_results.json` | A structured summary containing the group name, totals, active member, activity highlights, message types, contributors, words, emojis, selected personalities, and engagement score. |

## Requirements

- A Google account and [Google Colab](https://colab.research.google.com/)
- A ZIP archive containing a WhatsApp `.txt` chat export

The analytics portions use only Python's standard library (`re`, `os`, `zipfile`, `json`, `math`, `collections`, and `datetime`). The notebook uses Colab's upload, download, and SVG-display helpers only for notebook interaction and output display. It does not use pandas, NumPy, Matplotlib, Seaborn, or external analytics libraries.

## How to run

1. Open `vinay_GROUPDNA_WhatsApp_Analyzer (1).ipynb` in Google Colab.
2. Run the cells in notebook order. The upload cell will prompt you to select your WhatsApp export ZIP.
3. The notebook extracts the archive, searches it recursively for `.txt` files, and uses the first one it finds.
4. Continue running the analysis cells to calculate the metrics and build the dashboard.
5. Run the final cells to preview and download `GroupDNA_Dashboard.svg`, then save/download `GroupDNA_results.json`.

## Expected WhatsApp format

The primary extraction step expects message lines in this format:

```text
DD/MM/YYYY, HH:MM - Sender: Message
```

It also accepts `-` as the date separator and two- or four-digit years. Lines that do not begin with a matching message header are appended to the previous message, allowing multiline messages to be retained.

## Engagement score

The GroupDNA engagement score is a project-defined metric, not an official WhatsApp measure. It combines:

- **Activity score (70%)** — `(total messages / 3000) × 100`, capped at 100
- **Member score (30%)** — `(total members / 10) × 100`, capped at 100

The final weighted score is capped at 100 and labelled from `Quiet Group` through `Highly Active`.

## Current limitations

- The notebook uses the first `.txt` file found in the ZIP; it does not offer a chooser when several chat files are present.
- The core parser is tailored to the date/time and sender format above. Exports with a different locale, 12-hour time format, different separators, or different WhatsApp formatting may not parse completely.
- The main `messages` extraction only includes lines with a `Sender: Message` portion; system messages are not part of that analysis list.
- Date-based calculations treat dates as day/month/year, and analysis follows the source order rather than sorting messages first.
- Conversation starters are the first message on each date, not necessarily the first message after every conversational pause.
- Response-time analysis considers only adjacent messages from different senders and ignores gaps over 1,440 minutes.
- Emoji detection counts individual Unicode characters in the notebook's configured ranges; multi-codepoint emoji sequences may not be represented as one emoji.
- The SVG dashboard is assembled from variables created in earlier cells, so cells should be run sequentially in the same Colab runtime.

## Project structure

```text
vinay_GROUPDNA_WhatsApp_Analyzer (1).ipynb  # Colab notebook and complete workflow
GroupDNA_Dashboard.svg                      # Generated dashboard
GroupDNA_results.json                       # Generated results summary
```

## License

No license is specified in the notebook.
