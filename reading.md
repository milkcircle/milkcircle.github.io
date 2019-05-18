---
layout: page
title: Reading
permalink: /reading/
---

<!-- Include JQUERY stuff (I don't really get all this) -->
<script src="https://ajax.googleapis.com/ajax/libs/jquery/1.6.2/jquery.min.js"></script>
<script type="text/javascript" src="https://cdnjs.cloudflare.com/ajax/libs/jquery-easing/1.3/jquery.easing.min.js"></script>
<script type='text/javascript' src="https://cdnjs.cloudflare.com/ajax/libs/jquery-throttle-debounce/1.1/jquery.ba-throttle-debounce.min.js"></script>
<script type="text/javascript" src="/static/js/layout.js"></script>
<script type="text/javascript" src="/static/js/common.js"></script>
<script type="text/javascript"
    src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.1/MathJax.js?config=TeX-AMS-MML_HTMLorMML">
</script>

<body onload="start()">
<p>Below is a growing collection of literature (scientific and otherwise) I've read along with some summary thoughts.</p>

<center>
	<div class="showmore" id="showgeneticspapers" style="display:inline-block;">Genetics</div> 
  <div class="showmore" id="showhematologypapers" style="display:inline-block;">Hematology</div>
  <div class="showmore" id="showoncologypapers" style="display:inline-block;">Oncology</div>
  <div class="showmore" id="showpreventivepapers" style="display:inline-block;">Preventive</div>
  <div class="showmore" id="showbookpapers" style="display:inline-block;">Books</div>
</center>
<!-- <div id="sparse-ntm" style="display:none;"> -->

<div class="container">
  <div id="timeline">
    
  <div class="tyear">2019</div>

  <div id="bookpapers" class="timelineitem">
      <div class="tdate">May</div>
      <div class="ttitle" onClick="showDetails('exhalation')">
        Exhalation
        <a href="https://amzn.to/2VsW37H">
          <sup class="tlink">link</sup>
        </a>
      </div>
      <div id="exhalation" style="display:none;">
        <div class="tauthor">Ted Chiang</div>
        <div class="tcontent">
          <div class="timg_border"><img class="timage" src="/assets/papers/exhalation.jpg"></div>
        </div>
          <div class="tdesc">
            <p>
              Ted Chiang is a remarkable author who has lately earned wide recognition for his small collection of science fiction stories. His prior work, <a href="https://amzn.to/2HppjZ8">Stories of Your Life and Others</a>, led to the production of <i>Arrival</i>. This book contains 9 short stories varying from 3 pages in length to over 100, telling the tales of old and new societies grappling with harsh realities of humanity, technology, and free will. Stories such as "The Merchant and the Alchemist's Gate" and "The Lifecycle of Software Objects" already have the potential to be adapted into film.
            </p>
            <p>
              Perhaps the most interesting aspect of Chiang's writing is that it so often bucks the trend of science fiction writing. While he explores the themes of artificial intelligence and time travel, his stories are often set in ancient societies, giving his tales a flavor of fables; it is through this technique that he is able to dive more deeply into the humanity of his stories without getting distracted from the shiny settings of the future that so many other scifi writers indulge in. Though some of his stories are Black Mirror dark, others are less so and perhaps a little more optimistic. This new book comes with tremendously high recommendations. 9.5/10 from me.
            </p>
          </div>
        </div>
      </div>

  <div class="tyear">2018</div>
  
  <div id="preventivepapers" class="timelineitem">
      <div class="tdate">September</div>
      <div class="ttitle" onClick="showDetails('aspirin')">
        Effect of aspirin on disability-free survival in the healthy elderly
        <a href="/download/201809_aspirin_elderly.pdf">
          <sup class="tlink">link</sup>
        </a>
      </div>
      <div id="aspirin" style="display:none;">
        <div class="tauthor">J.J. McNeil, R.L. Woods, M.R. Nelson, C.M. Reid, B. Kirpach, R. Wolfe, E. Storey, R.C. Shah, J.E. Lockery, A.M. Tonkin, A.B. Newman, J.D. Williamson, K.L. Margolis, M.E. Ernst, W.P. Abhayaratna, N. Stocks, S.M. Fitzgerald, S.G. Orchard, R.E. Trevaks, L.J. Beilin, G.A. Donnan, P. Gibbs, C.I. Johnston, J. Ryan, B. Radziszewska, R. Grimm, and A.M. Murray, for the ASPREE Investigator Group</div>
        <div class="taffiliation">New England journal of medicine</div>
        <div class="tcontent">
          <div class="timg_border"><img class="timage" src="/assets/papers/aspirin_panel.png"></div>
        </div>
          <div class="tdesc">
            <p>
              <i>McNeil, et. al.</i> demonstrated over a 5-year span that low-dose aspirin therapy in healthy seniors did not prolong disability free survival, and led to a statistically significant increase in major hemorrhagic events. The cohort that was recruited included 19,114 persons, roughly half of which were randomized to receive 100 mg aspirin qday, and the other half of which received placebo. The trial was terminated at a median of 4.7 years of follow-up. There were 21.5 events per 1000 person-years in the aspirin group, compared to 21.2 events per 1000 person-years in the placebo group. Events were defined as death, dementia, or persistent physical disability. Limitations of this particular trial include a relatively short duration of intervention and a relatively old cohort of patients. Importantly, some participants had been regularly taking low-dose aspirin prior to the trial; the results of the paper did not stratify for these people, and it remains to be determined how the conclusion of this paper will contribute to the question of whether healthy elderly patients who had been using aspirin for primary prevention should continue or discontinue its use. Also notable was that the population in question was predominantly white.
            </p>
            <p>
              The authors of this study also released an accompanying study (<a href="/download/201809_aspirin_cancer.pdf">paper</a>) which showed, surprisingly, that the group of patients treated with aspirin had a statisically significant increase in the likelihood of cancer-related deaths, but not of deaths secondary to major hemorrhage. However, the aspirin group did experience a significantly higher rate of major hemorrhage events; these two points indicate that the hemorrhages did not overall contribute to an increase in the number of deaths.
            </p>
          </div>
        </div>
      </div>

  <div id="oncologypapers" class="timelineitem">
      <div class="tdate">September</div>
      <div class="ttitle" onClick="showDetails('aml_recurrence')">
        Identification of chemotherapy-induced leukemic-regenerating cells reveals a transient vulnerability of human AML recurrence
        <a href="/download/201809_recurrence_AML_vulnerability.pdf">
          <sup class="tlink">link</sup>
        </a>
      </div>
      <div id="aml_recurrence" style="display:none;">
        <div class="tauthor">Allison L. Boyd, Lili Aslostovar, Jennifer Reid, Wendy Ye, Borko Tanasijevic, Deanna P. Porras, Zoya Shapovalova, Mohammed Almakadi, Ronan Foley, Brian Leber, Anargyros Xenocostas, Mickie Bhatia</div>
        <div class="taffiliation">Cancer cell</div>
        <div class="tcontent">
          <div class="timg_border"><img class="timage" src="/assets/papers/aml_recurrence_panel.png"></div>
        </div>
          <div class="tdesc">
            <p>
              <i>Boyd, et. al.</i> demonstrate that, contrary to pre-existing theories, there does not exist a pool of leukemic stem cells (LSCs) that is resistant to chemotherapy, and that LSCs are equally susceptible to Ara-C as more downstream leukemic cells. Furthermore, they show that cytoreductive chemotherapy fuels accelerated leukemic regeneration to rates that exceed leukemic generation pre-chemotherapy. By the time the cancer has relapsed, leukemia-initiating cells and progenitor pools have already recovered. The authors took leukemia-regenerating cells (LRCs) post-chemotherapy and compared their gene expression signatures to that of untreated leukemic stem cells, and identified a set of uniquely upregulated genes that could be targeted with currently existing drugs. Treatment of the leukemia in xenografts with Ara-C alongside one of these drugs either completely wiped out the leukemia or delayed relapse significantly. Notably, the authors also found through their experimentation that leukemic regeneration cannot be modeled *in vitro*, suggesting an important role for the bone marrow microenvironment in leukemic regeneration. Finally, the authors describe a gene, SLC2A2, which acts as a reliable way to stratify patients based on disease remission vs. persistance/relapse.
            </p>
          </div>
        </div>
      </div>

  <div id="geneticspapers" class="timelineitem">
      <div class="tdate">July</div>
      <div class="ttitle" onClick="showDetails('primateai')">
        Predicting the clinical impact of human mutation with deep neural networks
        <a href="/download/201807_primateai.pdf">
          <sup class="tlink">link</sup>
        </a>
      </div>
      <div id="primateai" style="display:none;">
        <div class="tauthor">Laksshman Sundaram, Hong Gao, Samskruthi Reddy Padigepati, Jeremy F. McRae, Yanjun Li, Jack A. Kosmicki, Nondas Fritzilas, Jörg Hakenberg, Anindita Dutta, John Shon, Jinbo Xu, Serafim Batzloglou, Xiaolin Li, Kyle Kai-How Farh</div>
        <div class="taffiliation">Nature genetics</div>
        <div class="tcontent">
          <div class="timg_border"><img class="timage" src="/assets/papers/primateai_panel.png"></div>
        </div>
          <div class="tdesc">
            <p>
              <i>Sundaram, et. al.</i> published a deep neural network trained on a set of hundreds of thousands of common variants using a small population of 6 non-human primate species, and demonstrated that their new tool is powerful at classifying benign and pathogenic variants in humans. Notably, PrimateAI is unbiased compared to prior pathogenicity classifiers as it does not take human-classified ClinVar variants as input. PrimateAI also incorporates a secondary structure prediction model and solvent accessibility prediction model which takes as input a variant and its surrounding 102 amino acid sequence.
            </p>
          </div>
        </div>
      </div>

  <div id="hematologypapers" class="timelineitem">
      <div class="tdate">April</div>
      <div class="ttitle" onClick="showDetails('nets')">
        Increased neutrophil extracellular trap formation promotes thrombosis in myeloproliferative neoplasms
        <a href="/download/201804_nets_mpn_thrombosis.pdf">
          <sup class="tlink">link</sup>
        </a>
      </div>
      <div id="nets" style="display:none;">
        <div class="tauthor">Ofir Wolach, Rob S. Sellar, Kimberly Martinod, Deya Cherpokova, Marie McConkey, Ryan J. Chappell, Alexander J. Silver, Dylan Adams, Cecilia A. Castellano, Rebekka K. Schneider, Robert F. Padera, Daniel J. DeAngelo, Martha Wadleigh, David P. Steensma, Ilene Galinsky, Richard M. Stone, Giulio Genovese, Steven A. McCarroll, Bozenna Iliadou1, Christina Hultman1, Donna Neuberg, Ann Mullally, Denisa D. Wagner, Benjamin L. Ebert</div>
        <div class="taffiliation">Science translational medicine</div>
        <div class="tcontent">
          <div class="timg_border"><img class="timage" src="/assets/papers/net_panel.png"></div>
        </div>
          <div class="tdesc">
            <p>
              I made a presentation for my pediatric hematology rotation describing this paper in detail. The presentation can be found <a href="/download/201804_netosis_presentation.pdf">here</a>. Please note that there are several slides from the beginning of this slide deck that have been redacted for patient confidentiality.
            </p>
          </div>
        </div>
      </div>

  <div class="tyear">2017</div>

  <div id="geneticspapers" class="timelineitem">
      <div class="tdate">February</div>
      <div class="ttitle" onClick="showDetails('prs_cards')">
        Polygenic risk score identifies subgroup with higher burden of atherosclerosis and greater relative benefit from statin therapy in the primary prevention setting
        <a href="/download/201702_prs_cardiovascular.pdf">
          <sup class="tlink">link</sup>
        </a>
      </div>
      <div id="prs_cards" style="display:none;">
        <div class="tauthor">Pradeep Natarajan, Robin Young, Nathan O. Stitziel, Sandosh Padmanabhan, Usman Baber, Roxana Mehran, Samantha Sartori, Valentin Fuster, Dermot F. Reilly, Adam Butterworth, Daniel J. Rader, Ian Ford, Naveed Sattar, Sekar Kathiresan</div>
        <div class="taffiliation">Circulation</div>
        <div class="tcontent">
          <div class="timg_border"><img class="timage" src="/assets/papers/prs_cards_panel.png"></div>
        </div>
          <div class="tdesc">
            <p>
              <i>Natarajan, et. al.</i> describe developing a polygenic risk score from 57 SNPs which predicted additional benefit from statin therapy. This study demonstrated several important points. 1) Statins confer a greater risk reduction in those patients at high genetic risk, determined by the polygenic risk score. Interestingly this group does not on average have higher LDL levels compared to lower genetic risk subgroups. 2) Asymptomatic individuals with no history of coronary heart disease have higher burden of subclinical atherosclerosis with higher PRS. The PRS only incorporates common genetic variation but is not validated in patients with familial hypercholesterolemia.
            </p>
          </div>
        </div>
      </div>


  </div>


<script>
function start() {
	var show_genetics_papers = true;
  $("#showgeneticspapers").click(function() {
    if(!show_genetics_papers) {
      $('[id=geneticspapers]').each(function() {
      	$('[id=geneticspapers]').slideDown('fast', function() {
      		$("#showgeneticspapers").css('border', '2px solid #777');
          $("#showgeneticspapers").css('color', '#777');
      	})
      });
      show_genetics_papers = true;
    } else {
      $('[id=geneticspapers]').each(function() {
      	$('[id=geneticspapers]').slideUp('fast', function() {
      		$("#showgeneticspapers").css('border', '2px solid #CCC');
          $("#showgeneticspapers").css('color', '#CCC');
      	})
      });
      show_genetics_papers = false;
    }
  });

    var show_hematology_papers = true;
  $("#showhematologypapers").click(function() {
    if(!show_hematology_papers) {
      $('[id=hematologypapers]').each(function() {
        $('[id=hematologypapers]').slideDown('fast', function() {
          $("#showhematologypapers").css('border', '2px solid #777');
          $("#showhematologypapers").css('color', '#777');
        })
      });
      show_hematology_papers = true;
    } else {
      $('[id=hematologypapers]').each(function() {
        $('[id=hematologypapers]').slideUp('fast', function() {
          $("#showhematologypapers").css('border', '2px solid #CCC');
          $("#showhematologypapers").css('color', '#CCC');
        })
      });
      show_hematology_papers = false;
    }
  });

    var show_oncology_papers = true;
  $("#showoncologypapers").click(function() {
    if(!show_oncology_papers) {
      $('[id=oncologypapers]').each(function() {
        $('[id=oncologypapers]').slideDown('fast', function() {
          $("#showoncologypapers").css('border', '2px solid #777');
          $("#showoncologypapers").css('color', '#777');
        })
      });
      show_oncology_papers = true;
    } else {
      $('[id=oncologypapers]').each(function() {
        $('[id=oncologypapers]').slideUp('fast', function() {
          $("#showoncologypapers").css('border', '2px solid #CCC');
          $("#showoncologypapers").css('color', '#CCC');
        })
      });
      show_oncology_papers = false;
    }
  });

  	var show_preventive_papers = true;
  $("#showpreventivepapers").click(function() {
    if(!show_preventive_papers) {
      $('[id=preventivepapers]').each(function() {
      	$('[id=preventivepapers]').slideDown('fast', function() {
      		$("#showpreventivepapers").css('border', '2px solid #777');
          $("#showpreventivepapers").css('color', '#777');
      	})
      });
      show_preventive_papers = true;
    } else {
      $('[id=preventivepapers]').each(function() {
      	$('[id=preventivepapers]').slideUp('fast', function() {
      		$("#showpreventivepapers").css('border', '2px solid #CCC');
          $("#showpreventivepapers").css('color', '#CCC');
      	})
      });
      show_preventive_papers = false;
    }
  });

    var show_book_papers = true;
  $("#showbookpapers").click(function() {
    if(!show_book_papers) {
      $('[id=bookpapers]').each(function() {
        $('[id=bookpapers]').slideDown('fast', function() {
          $("#showbookpapers").css('border', '2px solid #777');
          $("#showbookpapers").css('color', '#777');
        })
      });
      show_book_papers = true;
    } else {
      $('[id=bookpapers]').each(function() {
        $('[id=bookpapers]').slideUp('fast', function() {
          $("#showbookpapers").css('border', '2px solid #CCC');
          $("#showbookpapers").css('color', '#CCC');
        })
      });
      show_book_papers = false;
    }
  });


}

</script>

<script type="text/javascript">

function showDetails(name) {
    $('#' + name).toggle(); 
}

// $(function(){
//   $('#ttitle').click(function(){
//      $('#xor_details').toggle(); 
//   });
// });
</script>