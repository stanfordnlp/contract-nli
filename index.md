ContractNLI is a dataset for document-level natural language inference (NLI) on contracts whose goal is to automate/support a time-consuming procedure of contract review.
In this task, a system is given a set of hypotheses (such as "Some obligations of Agreement may survive termination.") and a contract, and it is asked to classify whether each hypothesis is _entailed by_, _contradicting to_ or _not mentioned by_ (neutral to) the contract as well as identifying _evidence_ for the decision as spans in the contract.

![An overview of document-level NLI for contracts](resources/task_overview.png)

ContractNLI is the first dataset to utilize NLI for contracts and is also the largest corpus of annotated contracts (as of September 2021).
ContractNLI is an interesting challenge to work on from a machine learning perspective (the label distribution is imbalanced and it is naturally multi-task, all the while training data being scarce) and from a linguistic perspective (linguistic characteristics of contracts, particularly negations by exceptions, make the problem difficult).

Details of ContractNLI can be found in our [paper](https://aclanthology.org/2021.findings-emnlp.164/) that was published in "Findings of EMNLP 2021".
If you have a question regarding our dataset, you can contact us by emailing koreeda@stanford.edu or by creating an issue in this repository.

## Dataset specification

More formally, the task consists of:
* **Natural language inference (NLI)**: Document-level three-class classification (one of `Entailment`, `Contradiction` or `NotMentioned`).
* **Evidence identification**: Multi-label binary classification over _span_s, where a _span_ is a sentence or a list item within a sentence. This is only defined when NLI label is either `Entailment` or `Contradiction`. Evidence spans need not be contiguous but need to be comprehensively identified where they are redundant.

We have 17 hypotheses annotated on 607 non-disclosure agreements (NDAs).
The hypotheses are fixed throughout all the contracts including the test dataset.

Our dataset is provided as JSON files.

```json
{
  "documents": [
    {
      "id": 1,
      "file_name": "example.pdf",
      "text": "NON-DISCLOSURE AGREEMENT\nThis NON-DISCLOSURE AGREEMENT (\"Agreement\") is entered into this ...",
      "document_type": "search-pdf",
      "url": "https://examplecontract.com/example.pdf",
      "spans": [
        [0, 24],
        [25, 89],
        ...
      ],
      "annotation_sets": [
        {
          "annotations": {
            "nda-1": {
              "choice": "Entailment",
              "spans": [
                12,
                13,
                91
              ]
            },
            "nda-2": {
              "choice": "NotMentioned",
              "spans": []
            },
            ...
          }
        }
      ]
    },
    ...
  ],
  "labels": {
    "nda-1": {
      "short_description": "Explicit identification",
      "hypothesis": "All Confidential Information shall be expressly identified by the Disclosing Party."
    },
    ...
  }
}
```

The core information in our dataset is:
* `text`: The full document text
* `spans`: List of spans as pairs of the start and end character indices.
* `annotation_sets`: It is provided as a list to accommodate multiple annotations per document. Since we only have a single annotation for each document, you may safely access the appropriate annotation by `document['annotation_sets'][0]['annotations']`.
* `annotations`: Each key represents a hypothesis key. `choice` is either `Entailment`, `Contradiction` or `NotMentioned`. `spans` is given as indices of `spans` above. `spans` is empty when `choice` is `NotMentioned`.
* `labels`: Each key represents a hypothesis key. `hypothesis` is the hypothesis text that should be used in NLI.

The JSON file comes with supplemental information. Users may simply ignore the information if you are only interested in developing machine learning systems.
* `id`: A unique ID throughout train, development and test datasets.
* `file_name`: The filename of the original document in the dataset zip file.
* `document_type`: One of `search-pdf` (a PDF from a search engine), `sec-text` (a text file from SEC filing) or `sec-html` (an HTML file from SEC filing).
* `url`: The URL that we obtained the document from.


## Baseline system

In our paper, we introduced Span NLI BERT, a strong baseline for our task.
It (1) makes the problem of evidence identification easier by modeling the problem as multi-label classification over spans instead of trying to predict the start and end tokens, and (b) introduces more sophisticated context segmentation to deal with long documents.
We showed in our paper that Span NLI BERT significantly outperforms the existing models.

You can find the implementation of Span NLI BERT in [another repository](https://github.com/stanfordnlp/contract-nli-bert).

## License

Our dataset is released under CC BY 4.0.
Please refer attached "[LICENSE](./LICENSE)" or https://creativecommons.org/licenses/by/4.0/ for the exact terms.

When you use our dataset in your work, please cite our paper:

```bibtex
@inproceedings{koreeda-manning-2021-contractnli-dataset,
    title = "{C}ontract{NLI}: A Dataset for Document-level Natural Language Inference for Contracts",
    author = "Koreeda, Yuta  and
      Manning, Christopher",
    booktitle = "Findings of the Association for Computational Linguistics: EMNLP 2021",
    month = nov,
    year = "2021",
    address = "Punta Cana, Dominican Republic",
    publisher = "Association for Computational Linguistics",
    url = "https://aclanthology.org/2021.findings-emnlp.164",
    pages = "1907--1919",
    abstract = "Reviewing contracts is a time-consuming procedure that incurs large expenses to companies and social inequality to those who cannot afford it. In this work, we propose {``}document-level natural language inference (NLI) for contracts{''}, a novel, real-world application of NLI that addresses such problems. In this task, a system is given a set of hypotheses (such as {``}Some obligations of Agreement may survive termination.{''}) and a contract, and it is asked to classify whether each hypothesis is {``}entailed by{''}, {``}contradicting to{''} or {``}not mentioned by{''} (neutral to) the contract as well as identifying {``}evidence{''} for the decision as spans in the contract. We annotated and release the largest corpus to date consisting of 607 annotated contracts. We then show that existing models fail badly on our task and introduce a strong baseline, which (a) models evidence identification as multi-label classification over spans instead of trying to predict start and end tokens, and (b) employs more sophisticated context segmentation for dealing with long documents. We also show that linguistic characteristics of contracts, such as negations by exceptions, are contributing to the difficulty of this task and that there is much room for improvement.",
}
```

## Download

> Terms and Conditions of Use
>
> By clicking “DOWNLOAD” below and accessing, reviewing, downloading or otherwise making use of the Hitachi America, Ltd. dataset, entitled “ContractNLI: A Dataset for Document-level Natural Language Inference for Contracts” (the “Dataset”), or any portion thereof, you are agreeing to these Terms and Conditions of Use.
>
> If you do not agree to these Terms and Conditions of Use in their entirety, please do not access, review, download or otherwise make use of the Dataset.
>
> The Dataset is being made available to you pursuant to, and you understand and agree that you will have the rights to access, review, download or otherwise make use of the Dataset, in accordance with the terms and conditions of the Creative Commons Attribution 4.0 International Public License.
>
> THIRD PARTY WEBSITES, PLATFORMS AND NETWORKS: To the extent the Dataset contains any links to third party websites, social media platforms or other Internet-based networks, you understand and agree that Hitachi America, Ltd. does not own, operate, control or endorse such third party websites, platforms or networks.  Accordingly, these Terms and Conditions do not apply to any such third party websites, platforms or networks or govern such third parties’ collection, use, storage, disclosure, or other processing of your personal information.  Such third party websites, platforms or networks will be subject to their own terms and conditions of use and privacy policies.
>
> NOTICES OF COPYRIGHT INFRINGEMENT: In the event you believe that the Dataset incorporates any of your work or materials in a way that may constitute infringement of your copyright or any other intellectual property right, Hitachi America, Ltd. will take appropriate actions in response to your notification thereof.
>
> Pursuant to Title 17, United States Code, Section 512(c)(3), a notification of claimed infringement must be a written communication addressed to the designated agent as set forth below (the “Notice“), and must include substantially all of the following:
>
> (a) a physical or electronic signature of the person authorized to act on behalf of the owner of the copyright interest that is alleged to have been infringed;
>
> (b) a description of the copyrighted work or works that you claim have been infringed (“infringed work”) and identification of what material in such work(s) is claimed to be infringing (“infringing work”) and which you request to be removed or access to which is to be disabled;
>
> (c) a description of the exact name of the infringing work that is being used in the Dataset (and the location of the infringing work, if it appears in the Dataset);
>
> (d) information sufficient to permit Hitachi America, Ltd. to contact you, such as your physical address, telephone number, and email address;
>
> (e) a statement by you that you have a good faith belief that the use of the material identified in your Notice in the manner complained of is not authorized by the copyright owner, its agent, or the law;
>
> (f) a statement by you that the information in your Notice is accurate and, under penalty of perjury that you are the copyright owner or authorized to act on the copyright owner’s behalf.
>
> To reach Hitachi America, Ltd.’s Copyright Agent for Notice of claims of copyright infringement:
>
> Hitachi America, Ltd.
> 2535 Augustine Drive, Third Floor
> Santa Clara, California 95054
> Attn.: Legal Dept.
>
> The Copyright Agent should only be contacted if you believe that your work has been used or copied in a way that constitutes copyright infringement and such infringement is occurring on or through the Dataset. The Copyright Agent will not respond to any other inquiries.

**[DOWNLOAD](resources/contract-nli.zip)**

## Changelog and release note

* 10/5/2021: Initial release
* 12/13/2021: Added URL to ACL Anthology