# pyMap

pyMap is a simple interface for use with the NLM's MetaMap program. The idea and implementation originated from my needs to quickly process large amounts of medical text and convert it into a flattened table. As such, pyMap offers a utility for converting MetaMap XML output into a dictionary of lists (which can easily be converted to a Pandas DataFrame object). 

## Getting Started

### Dependencies

#### For pyMap

None! pyMap was designed to use nothing outside of the standard library.

#### For pyMap utils

* bs4
* pandas (recommended)
* lxml (recommended)

### Installation

If there is ever any interest, I'll create the necessary setup files.

## Usage

Instantiate a MetaMap class instance and process a sentence:

Reading:
```python
from pyMap import MetaMap

MM = MetaMap()

#Assuming that the MetaMap directory is already in your path
MM.start_skrmedpost_server('skrmedpostctl_start.bat')

text = 'He presented two hours later and a heart attack was ruled out by the physician.'
results = MM.process_text(text, 'metamap14.bat --XMLf')

#All done!
MM.stop_skrmedpost_server()
```

Optionally converting the XML results into a useable table:
```python
import pandas as pd
from utils import xml_to_soup, extract_results_from_soup

soup = xml_to_soup(results
out, extra_metadata = extract_results_from_soup(soup)
df = pd.DataFrame(out)
print(df)
```

df looks like:

| FullLexicalUnit   | Word      | LexicalCategory   | CUI      | SemanticType   | PreferredTerm         |   Negated |
|:------------------|:----------|:------------------|:---------|:---------------|:----------------------|----------:|
| He                | He        | pron              |          |                |                       |         0 |
| presented         | presented | verb              | C0449450 | idcn           | Presentation          |         0 |
| two               | two       | adj               | C0205448 | qnco           | Two                   |         0 |
| hours             | hours     | noun              | C0439227 | tmco           | Hour                  |         0 |
| later             | later     | adv               |          |                |                       |         0 |
| and               | and       | conj              |          |                |                       |         0 |
| a                 | a         | det               |          |                |                       |         0 |
| heart attack      | heart     | noun              | C0027051 | dsyn           | Myocardial Infarction |         1 |
| heart attack      | attack    | noun              | C0027051 | dsyn           | Myocardial Infarction |         1 |
| was               | was       | aux               |          |                |                       |         0 |
| ruled             | ruled     | verb              | C1446409 | qlco           | Positive              |         0 |
| out               | out       | adv               | C0439787 | spco           | Out (direction)       |         0 |
| by                | by        | prep              |          |                |                       |         0 |
| the               | the       | det               |          |                |                       |         0 |
| physician         | physician | noun              | C0031831 | prog           | Physicians            |         0 |
| .                 | .         | punc              |          |                |                       |         0 |

## Authors

David Beery

## License

[GNU GPLv3](https://www.gnu.org/licenses/gpl-3.0.en.html)

## Acknowledgements

*Thanks to Dr. Aronson and everyone at the NLM for creating and maintaining MetaMap in all its forms!