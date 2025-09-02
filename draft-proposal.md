Title: Reproducibility: Comparing the same procedure as Collberg and Proebsting on new data
Team Members: yuxuanm4
Problem: Up to 80 words describing the problem you want to work on.
Proposed solution: Up to 80 words describing how you want to solve it.
Evaluation: Up to 80 words describing how you will evaluate it.
[Optional] Notes: Some advice please
[Optional] Related paper: Can list one conference paper related to your project, not among those already 

# Feedback

The evaluation metric in the original study will have to be adapted a bit.

Start with Fig. 11 of the [Technical Report](https://web.archive.org/web/20220119115703/http://reproducibility.cs.arizona.edu/v2/RepeatabilityTR.pdf).

First, C&P exclude 106 papers (labelled "EX") because they didn't want to email the same authors twice. We aren't emailing authors, so we won't exclude these.

Second, we should decide if emailing and asking for source. We did not make this a requirement for MP2 because it could take a long time for the authors to respond. I haven't decided yet if it's reasonable to send emails for the semester long project.

Third, we should not count C&P OK_auth. OK_auth means that the authors claim the build should be repeatable, but C&P and their research assistants were not able to build the software. I think they caved in to pressure from the respondents in Sec 4.4.3, but they could not witness those being reproducible. If we don't email authors, we also won't be able to observe OK_auth.

What is the use in knowing the reproducibility statistics? Why bother studying it? I believe there are some uses, but you should try to flesh them out to make your project report stronger. We can discuss more later.

-- Sam
