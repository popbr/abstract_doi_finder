# Abstract & DOI Finder

A simple tool to retrieve <abbr title="Digital Object Identifier">DOI</abbr>s and abstracts of research articles inserted as spreadsheet.

## How-to Use

- First, we need some set-up:
    - [Prepare a spreadsheet](#spreadsheet-requirements), 
    - If you don't have an account on [github](https://github.com/), [create one](https://github.com/signup) and make sure you are logged-in.

- We will now create a copy of the program:
    - Go to <https://github.com/popbr/abstract_doi_finder/fork>, to "fork" (that is, create a copy of) our repository (that is, the program code),
    - Leave everything by default, and click on "Create fork":  
      > ![](how_to/fork_in_github.png)  
    - You should now see your own fork: you can make sure by checking that the address bar is of the form `https://github.com/<your-username>/abstract_doi_finder`, where `<your-username>` is your username (`aubertc` in the example below). 

- We will now upload our own spreadsheet.
    Click on the "abstract_doi_finder" folder:  
    > ![](how_to/naviguate_in_github_1.png)
    
    then click on the "input" folder, and click on the "Upload files" button hidden under the "Add file" button:  
    > ![](how_to/naviguate_in_github_2.png)

    Upload your spreadsheet, and click on "Commit changes":  
    > ![](how_to/naviguate_in_github_3.png)

    
- Now we will delete the spreadsheet loaded by default.

    Go back in the "input" folder, and click on the "test_input.xlsx" file. On the right of the screen, click on the three dots, then on "Delete file":  
    > ![](how_to/naviguate_in_github_4.png)
    
    Finally, click on "Commit changes…" twice:  
    > ![](how_to/naviguate_in_github_5.png)

- Now, we will execute the program on our spreadsheet and download the resulting spreadsheet.

    Click on "action", and then on "I understand my workflows, go ahead and enable them":  
    > ![](how_to/naviguate_in_github_6.png)
    
    Then, click on "Remote Execution", then on "Run workflow" (twice):  
    > ![](how_to/naviguate_in_github_7.png)
    
    Be patient, your workflow will be executed:  
    > ![](how_to/naviguate_in_github_8.png)
    
    Once it is over, a green check will replace the orange wheel. Click on it:  
    > ![](how_to/naviguate_in_github_9.png)

    Then, scroll down and click on "Remote_execution" under "Artifacts":  
    > ![](how_to/naviguate_in_github_10.png)
    
    That's it, your download of the resulting spreadsheet should begin!

  
> [!NOTE]
> If you want to run execute the program on a different sheet, simply upload your updated spreadsheet and head over to the "Actions" tab: if you there is only one sheet in the `input/` folder and the workflows have already been enabled, then they will execute automatically when the file in `input/` is edited.


## Spreadsheet Requirements

> [!CAUTION]
> Be aware that 
> 1. executing our program [as indicated above](#how-to-use) (that is, on github servers) will make your spreadsheet publicly accessible while the program is being executed, and after if you don't [delete your fork](https://docs.github.com/en/repositories/creating-and-managing-repositories/deleting-a-repository).
> 2. even if our program is not supposed to edit your spreadsheet, always backing up your data before running this program is a good practise.

Your speardsheet *must*
- use the `xlsx` format and extension,
- have at least one sheet containing
    - one column called "Author" or "Researcher" containing the name(s) of the author(s) (with or without capitals, singular or plural),
    - one column called "Title" containing the title of the paper (with or without capitals, singular or plural).

Your spreadsheet *can*
- have multiple sheets (all of them will be treated, except if [an argument](#arguments) limit the sheets to consider),
- have sheets that do not contain columns called "Author" or "Title" (they will be skipped),
- contain columns called "DOI" and "Abstract" (with or without capitals, singular or plural) -- those columns will be created if they are missing,

Your spreadsheet *must not*
- contain empty sheets (this may cause unforeseen errors).

> [!WARNING]  
> The program will not override your data, but create a copy of the original sheet. Furthermore, if you have data in existing cells for both the abstract and DOI, the program will not overwrite your data. However, if even one of them is missing, the program will search PubMed for the abstract and DOI, then overwrite the data in its output spreadsheet.

An example sheet that can be used as a template [is provided](https://github.com/popbr/abstract_doi_finder/blob/main/abstract_doi_finder/input/test_input.xlsx).


## How-to Compile and Execute

### Pre-requisites

- [Maven](https://maven.apache.org/install.html) (tested with Maven 3.9.6),
- Java (tested with Java 17.0.9),
- A [spreadsheet](#sheet-requirements).

### Compiling and Running the Program

```
cd abstract_doi_finder/
mvn compile
mvn exec:java -Dexec.mainClass="popbr.AbstractDoiFinder" -Dexec.args="input_file.xlsx 1,3"
```

#### Arguments

In the command line above, 

- `input_file.xlsx` is the name of the spreadsheet placed in the `abstract_doi_finder/input/` folder (this argument is optional if only one sheet is in the `input/` folder, mandatory otherwise), and
- `1,3` are the sheets you want to run the program on. Please separate the values with commas, exclude spaces, or follow the examples below.

The range can be provided (or omitted) in a multitude of ways including:

- "*" can be used to run the program on all sheets. This is also the default if no sheet range is provided.
- "1,3" would run the program on sheets 1 and 3,
- "4-10" would run the program on sheets 4, 5, 6, 7, 8, 9, and 10,
- "*-3" would run the program on sheets 1, 2 and 3.
- "10-*" would run the program from sheet 10 to the last sheet.
- When entering an asterisk ('*') for the sheet range, it starts with the very first sheet in your excel file. (At index 0)

> [!NOTE]  
> When entering your sheet range, it is important to take note that each number is subtracted by 1, so it runs on your intended files.
> For example: The user inputs '1,2,3' which would run on the first sheet, the second sheet, and the third.
> But the program will use the indices 0, 1, and 2 to run on the specified sheets.
>  Therefore, it is important to not use 0, as this was done for user convenience.
