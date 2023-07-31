```
\chapter{Requirements Analysis and Design}
\label{chap:analysis}

\section{Introduction}
The previous chapter presented the general context. This chapter titled "Requirements Analysis and Design" will identify the system's actors, the requirements and system design accompanied with UML diagrams.

\subsection{Requirements analysis}
Pin pointing the requirements allows a smooth flow of the execution and sets a great foundation for designing the application workflow.
We will determine the functional and non functional requirements of the solution and its actors.

\subsection{Actor identification}
An actor is by definition an entity who externally interacts with the system. We distinguish two type of actors:
\begin{itemize}
    % TODO COPIED
    \item \textbf{Primary actors:} also called active actors who interacts directly with the system use cases in order to achieve a certain goal.

    % TODO COPIED
    \item \textbf{Secondary actors:} also known as passive actors who are used by the system to achieve a certain goal but they don't interact with the application on their own.
\end{itemize}

For our specific case, the active actor is a developer who wishes to benchmark the caching strategies along with eviction policies for his own specific case. He should be able to implement certain functions that depend on the use case.

\subsection{Functional requirements}
Functional requirements specifies what the application has to offer in terms of services and functionality. In short, it specifies what functions the application must implement.

Promo planner application has its own actor in reality which is the retailer. The promo planner application should expose specific and secure endpoints to the user, which are called APIs. Through which, the user could read and write data in the application.

Should start explaning the implemntation liek dis: implementation without caching, implementing just the main application and cassandra then add caching layer and benchmarks that's how in reality it went

```
