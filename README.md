We need python 3.6 environment, we can use Conda to config.

# Creating a Conda Environment

Run the following command.

    conda create --name cmpt310 python=3.6
    conda activate cmpt310

### Pacman Problem

ThispartisadaptedfromUCBerkeleyandUW.

#### Introduction

Inthisproject,youwilldesignagentsfortheclassicversionofPacman,includingghosts.
Along the way, you will implement both minimax and expectimax search and try your
handatevaluationfunctiondesign.

The code base has not changed much from the previous project, but please start with
afreshinstallation,ratherthaninterminglingfilesfromproject1.

As in project 1, this project includes an autograder for you to grade your answers on
yourmachine. Thiscanberunonallquestionswiththecommand:

python autograder.py

Note: IfyourpythonreferstoPython2.7,youmayneedtoinvokepython3autograder.py
(and similarly for all subsequent Python invocations) or create a conda environment as
describedinProject0.

Itcanberunforoneparticularquestion,suchasq2,by:

python autograder.py -q q

Itcanberunforoneparticulartestbycommandsoftheform:

python autograder.py -t test_cases/q2/0-small-tree

By default, the autograder displays graphics with the -t option, but doesn’t with the -q
option. Youcanforcegraphicsbyusingthe–graphicsflag,orforcenographicsbyusing
the–no-graphicsflag.

SeetheautogradertutorialinProject0formoreinformationaboutusingtheautograder.

YoucandownloadallthecodeandsupportingfilesasaziparchivefromFiles/Assignment
2/multiagent.zip.

Thecodeforthisprojectcontainsthefollowingfiles:

Filesyou’lledit:

- multiAgents.py→Whereallofyourmulti-agentsearchagentswillreside.

Filesyoumightwanttolookat:


- pacman.py→The main file that runs Pacman games. This file also describes a
    PacmanGameStatetype,whichyouwilluseextensivelyinthisproject.
- game.py→The logic behind how the Pacman world works. This file describes
    severalsupportingtypeslikeAgentState,Agent,Direction,andGrid.
- util.py→Useful data structures for implementing search algorithms. You don’t
    needtousetheseforthisproject,butmayfindotherfunctionsdefinedheretobe
    useful.

Supportingfilesyoucanignore:

- graphicsDisplay.py→GraphicsforPacman
- graphicsUtils.py→SupportforPacmangraphics
- textDisplay.py→ASCIIgraphicsforPacman
- ghostAgents.py→Agentstocontrolghosts
- keyboardAgents.py→KeyboardinterfacestocontrolPacman
- layout.py→Codeforreadinglayoutfilesandstoringtheircontents
- autograder.py→Projectautograder
- testParser.py→Parsesautogradertestandsolutionfiles
- testClasses.py→Generalautogradingtestclasses
- test_cases/Directorycontainingthetestcasesforeachquestion
- searchTestClasses.py→Project2specificautogradingtestclasses.

### Welcome to Multi-Agent Pacman

First,playagameofclassicPacmanbyrunningthefollowingcommand:

python pacman.py

andusingthearrowkeystomove. Now,runtheprovidedReflexAgentinmultiAgents.py

python pacman.py -p ReflexAgent

Notethatitplaysquitepoorlyevenonsimplelayouts:

python pacman.py -p ReflexAgent -l testClassic

Inspectitscode(inmultiAgents.py)andmakesureyouunderstandwhatit’sdoing.


### Question 1 (4 points): Reflex Agent

Improve the _ReflexAgent_ inmultiAgents.pyto play respectably. The provided reflex
agentcodeprovidessomehelpfulexamplesofmethodsthatquerythe _GameState_ for
information. Acapablereflexagentwillhavetoconsiderbothfoodlocationsandghost
locations to perform well. Your agent should easily and reliably clear the _testClassic_
layout:

python pacman.py -p ReflexAgent -l testClassic

Try out your reflex agent on the default _mediumClassic_ layout with one ghost or two
(andanimationofftospeedupthedisplay):

python pacman.py --frameTime 0 -p ReflexAgent -k 1
python pacman.py --frameTime 0 -p ReflexAgent -k 2

How does your agent fare? It will likely often die with 2 ghosts on the default board,
unlessyourevaluationfunctionisquitegood.

Note:Asfeatures,trythereciprocalofimportantvalues(suchasdistancetofood)rather
thanjustthevaluesthemselves.

Note: The evaluation function you’re writing is evaluating state-action pairs; in later
partsoftheproject,you’llbeevaluatingstates.

Note:Youmayfinditusefultoviewtheinternalcontentsofvariousobjectsfordebug-
ging. Youcandothisbyprintingtheobjects’stringrepresentations. Forexample,you
canprint _newGhostStates_ withprint(str(newGhostStates)).

Options:Default ghosts are random; you can also play for fun with slightly smarter di-
rectionalghostsusing-g DirectionalGhost. Iftherandomnessispreventingyoufrom
tellingwhetheryouragentisimproving,youcanuse-ftorunwithafixedrandomseed
(samerandomchoiceseverygame). Youcanalsoplaymultiplegamesinarowwith-n.
Turnoffgraphicswith-qtorunlotsofgamesquickly.

Grading:WewillrunyouragentontheopenClassiclayout10times. Youwillreceive
pointsifyouragenttimesout,orneverwins. Youwillreceive1pointifyouragentwins
atleast5times,or2pointsifyouragentwinsall10games. Youwillreceiveanaddition
1pointifyouragent’saveragescoreisgreaterthan500,or2pointsifitisgreaterthan

1000. Youcantryyouragentoutundertheseconditionswith

python autograder.py -q q

Torunitwithoutgraphics,use:

python autograder.py -q q1 --no-graphics

Don’t spend too much time on this question, though, as the meat of the project lies
ahead.


### Question 2 (5 points): Minimax

NowyouwillwriteanadversarialsearchagentintheprovidedMinimaxAgentclassstub
inmultiAgents.py. Your minimax agent should work with any number of ghosts, so
you’llhavetowriteanalgorithmthatisslightlymoregeneralthanwhatyou’vepreviously
seen in lecture. In particular, your minimax tree will have multiple min layers (one for
eachghost)foreverymaxlayer.

Your code should also expand the game tree to an arbitrary depth. Score the leaves
of your minimax tree with the suppliedself.evaluationFunction, which defaults to
scoreEvaluationFunction.MinimaxAgentextendsMultiAgentSearchAgent,whichgives
access toself.depthandself.evaluationFunction. Make sure your minimax code
makesreferencetothesetwovariableswhereappropriateasthesevariablesarepopu-
latedinresponsetocommandlineoptions.

Important:AsinglesearchplyisconsideredtobeonePacmanmoveandalltheghosts’
responses,sodepth2searchwillinvolvePacmanandeachghostmovingtwotimes.

Grading:We will be checking your code to determine whether it explores the correct
numberofgamestates. Thisistheonlyreliablewaytodetectsomeverysubtlebugsin
implementations of minimax. As a result, the autograder will be very picky about how
many times you callGameState.generateSuccessor. If you call it any more or less than
necessary,theautograderwillcomplain. Totestanddebugyourcode,run

python autograder.py -q q

Thiswillshowwhatyouralgorithmdoesonanumberofsmalltrees,aswellasapacman
game. Torunitwithoutgraphics,use:

python autograder.py -q q2 --no-graphics

#### Hints and Observations

- The correct implementation of minimax will lead to Pacman losing the game in
    sometests. Thisisnotaproblem: asitiscorrectbehaviour,itwillpassthetests.
- TheevaluationfunctionforthePacmantestinthispartisalreadywritten(self.evaluationFunction).
    Youshouldn’tchangethisfunction,butrecognizethatnowwe’reevaluatingstates
    rather than actions, as we were for the reflex agent. Look-ahead agents evaluate
    futurestateswhereasreflexagentsevaluateactionsfromthecurrentstate.
- TheminimaxvaluesoftheinitialstateintheminimaxClassiclayoutare9,8,7,-
    fordepths1,2,3and4respectively. Notethatyourminimaxagentwilloftenwin
    (665/1000gamesforus)despitethedirepredictionofdepth4minimax.

```
python pacman.py -p MinimaxAgent -l minimaxClassic -a depth=
```
- Pacmanisalwaysagent0,andtheagentsmoveinorderofincreasingagentindex.


- AllstatesinminimaxshouldbeGameStates,eitherpassedintogetActionorgener-
    atedviaGameState.generateSuccessor. Inthisproject,youwillnotbeabstracting
    tosimplifiedstates.
- OnlargerboardssuchasopenClassicandmediumClassic(thedefault),you’llfind
    Pacman to be good at not dying, but quite bad at winning. He’ll often thrash
    around without making progress. He might even thrash around right next to a
    dotwithouteatingitbecausehedoesn’tknowwherehe’dgoaftereatingthatdot.
    Don’tworryifyouseethisbehavior,question5willcleanupalloftheseissues.
- WhenPacmanbelievesthathisdeathisunavoidable, hewilltrytoendthegame
    as soon as possible because of the constant penalty for living. Sometimes, this
    isthewrongthingtodowithrandomghosts,butminimaxagentsalwaysassume
    theworst:

```
python pacman.py -p MinimaxAgent -l trappedClassic -a depth=
```
```
MakesureyouunderstandwhyPacmanrushestheclosestghostinthiscase.
```
### Question 3 (5 points): Alpha-Beta Pruning

Makeanewagentthatusesalpha-betapruningtomoreefficientlyexploretheminimax
tree, inAlphaBetaAgent. Again, your algorithm will be slightly more general than the
pseudocodefromlecture,sopartofthechallengeistoextendthealpha-betapruning
logicappropriatelytomultipleminimizeragents.

Youshouldseeaspeed-up(perhapsdepth3alpha-betawillrunasfastasdepth2min-
imax). Ideally, depth 3 onsmallClassicshould run in just a few seconds per move or
faster.

pythonpacman.py-pAlphaBetaAgent-adepth=3-lsmallClassic

TheAlphaBetaAgentminimax values should be identical to theMinimaxAgentminimax
values,althoughtheactionsitselectscanvarybecauseofdifferenttie-breakingbehav-
ior. Again, the minimax values of the initial state in theminimaxClassiclayout are 9, 8,
7and-492fordepths1,2,3and4respectively.

Grading: Because we check your code to determine whether it explores the correct
numberofstates,itisimportantthatyouperformalpha-betapruningwithoutreordering
children. In other words, successor states should always be processed in the order re-
turnedbyGameState.getLegalActions. Again,donotcallGameState.generateSuccessor
morethannecessary.

You must not prune on equality in order to match the set of states explored by our
autograder.(Indeed, alternatively, but incompatible with our autograder, would be to
alsoallowforpruningonequalityandinvokealpha-betaonceoneachchildoftheroot
node,butthiswillnotmatchtheautograder.)


Thepseudo-codebelowrepresentsthealgorithmyoushouldimplementforthisques-
tion.

Totestanddebugyourcode,run

python autograder.py -q q

Thiswillshowwhatyouralgorithmdoesonanumberofsmalltrees,aswellasapacman
game. Torunitwithoutgraphics,use:

python autograder.py -q q3 --no-graphics

The correct implementation of alpha-beta pruning will lead to Pacman losing some of
thetests. Thisisnotaproblem: asitiscorrectbehaviour,itwillpassthetests.

### Question 4 (5 points): Expectimax

Minimaxandalpha-betaaregreat,buttheybothassumethatyouareplayingagainstan
adversarywhomakesoptimaldecisions. Asanyonewhohaseverwontic-tac-toecantell
you,thisisnotalwaysthecase. InthisquestionyouwillimplementtheExpectimaxAgent,
whichisusefulformodelingprobabilisticbehaviorofagentswhomaymakesuboptimal
choices.

Aswith the searchand constraintsatisfactionproblemscoveredso farin this class, the
beauty of these algorithms is their general applicability. To expedite your own devel-
opment, we’ve supplied some test cases based on generic trees. You can debug your
implementationonsmallthegametreesusingthecommand:

python autograder.py -q q

Debugging on these small and manageable test cases is recommended and will help
youtofindbugsquickly.

Once your algorithm is working on small trees, you can observe its success in Pacman.
Randomghostsareofcoursenotoptimalminimaxagents,andsomodelingthemwith


```
minimaxsearchmaynotbeappropriate.ExpectimaxAgent,willnolongertakethemin
overallghostactions,buttheexpectationaccordingtoyouragent’smodelofhowthe
ghostsact. Tosimplifyyourcode,assumeyouwillonlyberunningagainstanadversary
whichchoosesamongsttheirgetLegalActionsuniformlyatrandom.
ToseehowtheExpectimaxAgentbehavesinPacman,run:
```
```
python pacman.py -p ExpectimaxAgent -l minimaxClassic -a depth=
```
```
You should now observe a more cavalier approach in close quarters with ghosts. In
particular,ifPacmanperceivesthathecouldbetrappedbutmightescapetograbafew
morepiecesoffood,he’llatleasttry. Investigatetheresultsofthesetwoscenarios:
```
```
python pacman.py -p AlphaBetaAgent -l trappedClassic -a depth=3 -q -n 10
python pacman.py -p ExpectimaxAgent -l trappedClassic -a depth=3 -q -n 10
```
```
YoushouldfindthatyourExpectimaxAgentwinsabouthalfthetime,whileyourAlphaBetaAgent
alwaysloses. Makesureyouunderstandwhythebehaviorherediffersfromtheminimax
case.
ThecorrectimplementationofexpectimaxwillleadtoPacmanlosingsomeofthetests.
Thisisnotaproblem: asitiscorrectbehaviour,itwillpassthetests.
```
### Question 5 (6 points): Evaluation Function

WriteabetterevaluationfunctionforpacmanintheprovidedfunctionbetterEvaluationFunction.
Theevaluationfunctionshouldevaluatestates,ratherthanactionslikeyourreflexagent
evaluationfunctiondid. Youmayuseanytoolsatyourdisposalforevaluation,including
your search code from the last project. With depth 2 search, your evaluation function
should clear thesmallClassiclayout with one random ghost more than half the time
andstillrunatareasonablerate(togetfullcredit,Pacmanshouldbeaveragingaround
1000pointswhenhe’swinning).
Grading:the autograder will run your agent on the smallClassic layout 10 times. We
willassignpointstoyourevaluationfunctioninthefollowingway:
If you win at least once without timing out the autograder, you receive 1 points. Any
agentnotsatisfyingthesecriteriawillreceive0points. +1forwinningatleast5times,+
forwinningall10times+1foranaveragescoreofatleast500,+2foranaveragescoreof
atleast1000(includingscoresonlostgames)+1ifyourgamestakeonaveragelessthan
30secondsontheautogradermachine,whenrunwith--no-graphics. Theautograder
is run on attu, so this machine will have a fair amount of resources, but your personal
computercouldbefarlessperformant(netbooks)orfarmoreperformant(gamingrigs).
The additional points for average score and computation time will only be awarded if
youwinatleast5times. Youcantryyouragentoutundertheseconditionswith

```
python autograder.py -q q
```

Torunitwithoutgraphics,use:

python autograder.py -q q5 --no-graphics

### Submission

In order to submit your project, please uploadmultiAgents.pyfile to Canvas. Please
donotuploadthefilesinazipfileoradirectory.
