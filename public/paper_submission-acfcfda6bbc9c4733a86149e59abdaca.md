# Paper Submission

## JOSS Instruction

Here in this chapter, we will briefly describe how we follow [Submitting a paper to JOSS](https://joss.readthedocs.io/en/latest/submitting.html).

## Markdown Source Text

The draft paper is supposed to be prepared as a markdown source text, named "paper.md", which should be placed in the [joss-paper branch](https://github.com/biosaxs-dev/molass-library/tree/joss-paper) of the [code repository](https://github.com/biosaxs-dev/molass-library). It should follow [JOSS Paper Format](https://joss.readthedocs.io/en/latest/paper.html).

## PDF Compilation

To check the format, JOSS provides a script for GitHub Actions[^1], which automates the markdown source text to PDF compilation.
The execution of the script will be triggered if you `"push"` the changes of "paper.md". After its completion, you can retrieve a compiled PDF from the GitHub Actions job result.

[^1]: See [Open Journals PDF Generator](https://github.com/marketplace/actions/open-journals-pdf-generator). As suggested there, we have placed this script to .github/workflows/draft-pdf.yml in the repository branch.

## Paper Submission

See [Submission Process](https://joss.readthedocs.io/en/latest/submitting.html#submission-process).