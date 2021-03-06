# ScribanCsvTemplateEngine
### ScribanCsvTemplateEngine - text (code) templating from csv file input ...featuring Scriban!
_previously YamlCodeGenThing_

1) Take a bunch of template text files, using {{ }} for variable placement (Scriban style) for values to be exchanged.

2) Take a csv file, with the first row being csv headers to give the columns names, and then these names can be referenced in the template corpus as exchange variables, e.g: 
```
  "age,  name,   origin_story,
  27, John, Blah blah,
  32, Jill, Etc etc"
```
And in the template file:
```
  "Their age is {{age}}, and their name is {{name}.
  {{origin_story}}"
```     
3) This command line utility then creates a _*copy of each* of the template files for each row in the csv file_, exchanging the values using Scriban.


In addition to csv data variables, data variables can also be added globally. These are put in the yaml file, and add to the model for each csv row.
The global data lives in the 'data:' section of the yaml (see below)

These yaml data variables are added to the csv model for each row. Note that data items in the csv file will override data items in the global yaml data, if they share the same name. 

Here is an example yaml file. The free form values are directly under the 'data:' section:

```#Example program yaml
input:
  csv_filename: .\Example\input.csv
template:
  scriban_filenames:
    - .\Example\Templates\test1.cs
    - .\Example\Templates\test2.cs
    - .\Example\Templates\test3.cs
    - .\Example\Templates\Subfolder\test4.cs
  base_path: .\Example\Templates\
output:
  output_path: .\Example\Output\
  csv_column_for_ouput_path_part: name #<-- references the csv header from the input file, and uses this to build the output path to separate the results for each csv row. Spaces in names are stripped
data:
  add_your_extra_data_items_here_which_are_accessable_by_all_template_files:
    using_standard_yaml: true
  which_means_you_can_make_lists:
    - like_this
    - and_this
    - etc
  and:
    nested:
      data_items:
         - name: bob
           age: 50
           fav_colour: blue
         - name: alf
           age: 70
           fav_colour: red
  more_data:
    and_more: A Little Lamb
```
    

Then you can reference this data in the template files, using Scriban notation: 

  `<html><body>{{for item in which_means_you_can_make_lists}}`
  
Or

  `Mary had {{more_data.and_more}}`

Template file data overrides:
Data items can also be added for each template file. This is done by adding a .yaml file of the same name as the template file, except changing the extension to be 'yaml' instead of what it was originally, and putting this yaml file in the same folder as the template file of the same name. You do not need a 'data:' section for these template override yaml files, as the whole yaml is added as the model. 

Order of precedence, amongst the different data source, for data members name clashes is:

1st - Csv data items   (if the value is blank, it is ignored and the value is taken from the next in order of precedence. Only blank csv data items give way to lesser presidence.
2nd - ScribanCsvTemplateEngine.yaml 'data:'
3rd - template file yaml data

That is, main program yaml 'data:' items will override the template level yaml.

And csv data will override all others.
