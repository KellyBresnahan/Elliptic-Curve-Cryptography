# Elliptic-Curve-Cryptography
This should hopefully turn out to be a copy of my Beamer presentation on Elliptic Curve Cryptography

\documentclass{beamer}
\usepackage{amsmath, bookmark,enumerate, xcolor}
\usepackage{caption}
\definecolor{Magenta}{cmyk}{0,1,0,0}
\usetheme{PaloAlto} 
\usecolortheme{crane}
\pdfmapfile{+sansmathaccent.map} 
\newcommand{\BI}[1]{\textbf{\textit{#1}}}
\newcommand{\Mod}[1]{\ (\text{mod}\ #1)}
\newcommand{\floor}[1]{\left \lfloor #1 \right \rfloor}
%References: http://en.wikibooks.org/wiki/LaTeX/Presentations
%http://deic.uab.es/~iblanes/beamer_gallery/

\title{Elliptic Curve Cryptography}
\subtitle{}
\author{Kelly Bresnahan}
\date{March 24, 2016}


\begin{document}
        \setbeamertemplate{sidebar left}{}
		\begin{frame}
			\titlepage
		\end{frame}

		\begin{frame}
		\subsection{Table of Contents}
			\frametitle{Table Of Contents}
			\begin{enumerate}
			\item Elliptic Curve Cryptography (ECC)
			\begin{itemize}
			\item Introduction
			\item Pros and Cons of Elliptic Curves
			\item Definition of an Elliptic Curve
			\item Operations on Elliptic Curves
			\item Hasse's Bound 
			\item Representing Plaintext
			\item Elliptic Curve Diffie-Hellman Key Exchange
			\item ElGamal Digital Signatures using Elliptic Curves
			\item Identity-Base Encryption Using ECC
			\end{itemize}
            \end{enumerate}						
		\end{frame}

			\begin{frame}
				\subsection{Introduction}
				\frametitle{Introduction}
				\begin{itemize}
				\item Miller and Koblitz (independently) introduced elliptic curves into cryptography in 						the mid-1980s
				\item Elliptic Curve Cryptography algorithms entered wide use between 2004 and 2005
				%\item \href{https://en.wikipedia.org/wiki/Penis_transplantation}{Click here for an example of its wide spread use}
				\item Based on the \textbf{discrete logarithm problem}, i.e. 
				%Given a prime $p$, and two nonzero elements $g$ and $b$ $\Mod{p}$, the discrete log problem is the problem of 
				determining an integer $1 \leq k \leq p-1$ such that
			$$g^k = b \Mod {p}$$  
				\end{itemize}								 
					
			\end{frame}
			
			\begin{frame}
				\subsection{Pros and Cons}
				\frametitle{Why use ECC?}
				\begin{itemize}
				\item \textbf{Pros}
				\begin{itemize}
				\pause
				\item Smaller keys can be used to achieve the same security as an RSA or discrete 								logarithm system
				\pause
				\begin{itemize}
				\item ~ 160-256 bit vs 1024-3072 bit
				\end{itemize}
				\pause
				\item Only generic attacks are known against ECC in comparison to other systems such as 						RSA and discrete logarithm (DL) schemes
				\pause
				\item ECDSA signature with a 256-bit key is over 20 times faster than an RSA signature with a 2,048-bit key 
				\pause
				\item The energy needed to break an RSA key is much smaller than an ECC key
				\pause
				\end{itemize}
				\item \textbf{Cons}
				\pause
				\begin{itemize}
				\item Security is achieved only if cryptographically strong elliptic curves are used
				\end{itemize}
				\end{itemize}
				
				\iffalse
			To visualize how much harder it is to break, Lenstra recently introduced the concept of "Global Security." You can compute how much energy is needed to break a cryptographic algorithm and compare that with how much water that energy could boil. This is a kind of a cryptographic carbon footprint. By this measure, breaking a 228-bit RSA key requires less energy than it takes to boil a teaspoon of water. Comparatively, breaking a 228-bit elliptic curve key requires enough energy to boil all the water on earth. For this level of security with RSA, you'd need a key with 2,380 bits.
				\fi
			\end{frame}

			\begin{frame}
				\subsection{What is an Elliptic Curve?}
				\frametitle{Definition of Elliptic Curves}
					\textbf{Definition:} An \BI{elliptic curve} is the graph of the equation
					$$E: y^2 = x^3 + ax^2+ bx + c$$
					where $a$, $b$, and $c$ are elements from the base field $K$ of characteristic not 							equal to 2. \\
					\  \\
					\textit{Note:} We'll also include the point $(\infty, \infty)$, denoted $\infty$ \\
					 
			\end{frame}
			
			\begin{frame}
				\subsection{Examples of Elliptic Curves over $\mathbb{R}$}
				\frametitle{Examples of Elliptic Curves over $\mathbb{R}$}
					\begin{figure}
					\centering
					\begin{minipage}{.5\textwidth}
				  		\centering
				  		\includegraphics[scale=0.5]{../EllipticCurveOne.png}
				  		\captionof{figure}{$y^2 = x^3 + x$}
						\end{minipage}%
					\begin{minipage}{.5\textwidth}
				  		\centering
  						\includegraphics[scale=0.48]{../EllipticCurveTwo.png}
  						\captionof{figure}{$y^2 = x^3 + 73$}
					\end{minipage}
					\end{figure}
			\end{frame}
			
			\begin{frame}
				\subsection{Point Addition}
				\frametitle{Operations on Elliptic Curves}
				\textbf{Point Addition} \\
				\begin{figure}[!h]
				\centering
				\includegraphics[scale=0.5]{../PointAdditionECC.png}
				\end{figure}
					
			\end{frame}
			
			
			\begin{frame}
			\subsection{Point Doubling}
			\frametitle{Operations on Elliptic Curves (cont)}
				\textbf{Point Doubling} \\
				\begin{figure}[!h]
				\centering
				\includegraphics[scale=0.5]{../PointDoublingECC.png}
				\end{figure}
			\end{frame}
			
			
			\begin{frame}
				\subsection{Adding Infinity}
				\frametitle{Operations on Elliptic Curves (cont)}
				\textbf{How do we add a point $P$ with $\infty$?}
				\pause
				\begin{figure}[!h]
				\centering
				\includegraphics[scale=0.45]{../AddingInfinityECC.png}
				\end{figure}
								
			\end{frame}
			
			
			\begin{frame}
				\subsection{Group Operations on Elliptic Curves: The Group}
				\frametitle{Operations on Elliptic Curves (cont)}
				Therefore, the points on $E$ form an abelian group under addition where 
				\begin{enumerate}
				\item $\infty$ is the additive identity
				\item The inverse of the point $P = (x, y)$ is $-P = (x, -y)$
				\item $P - Q = P + (-Q)$
				%\item $P + Q + R = \infty$ if and only if $P$, $Q$, and $R$ are colinear
				\end{enumerate}
			\end{frame}
			
			
			\begin{frame}
			\subsection{Seeing a Curve mod p}
			\frametitle{Elliptic Curve in $\mathbb{R}$}
				\begin{figure}[!h]
				\centering
				\includegraphics[scale=0.45]{../EllipticCurveThree.png}
				\end{figure}
			\end{frame}
			
			
			\begin{frame}
			\frametitle{Same Curve $\Mod{p}$}
				\begin{figure}[!h]
				\centering
				\includegraphics[scale=0.45]{../EllipticCurveModp.png}
				\end{figure}
			\end{frame}
			
			
			\begin{frame}
			\subsection{Example of Adding Points on E}
			\frametitle{Adding Points on E}
			Suppose $E$ is defined as $y^2 \equiv x^3 + 4x + 4 \Mod{5}$. \\
			Let $P_1 = (1,2)$ and $P_2 = (4, 3)$. Then $$(1, 2) + (4, 3) = (4, 2)$$
			%This works by hand, I checked. I got this from Washington and Trappe's book
			\end{frame}
			
			
			\begin{frame}
			\subsection{Example of Point Doubling}
			\frametitle{Doubling Points on P}
			Suppose $E$ is defined as $y^2 \equiv x^3 + 2x + 2 \Mod{17}$. \\ Let $P = (5,1)$. Then
			$$2P = (6, 3)$$
			%note: this example came from page 245 of Paar and Pelzl's book. I verified that it works
			\end{frame}
			
			
			\begin{frame}
				\subsection{Addition Law}
				\frametitle{Addition Law}
				If $E$ is given by $E: y^2 = x^3 + bx + c \Mod{p}$ we define 
				$$(x_3, y_3)= (x_1, y_1) + (x_2, y_2)$$ as
				\begin{align*}
				x_3 &= s^2 - x_1 - x_2 \Mod{p} \text{\hspace{2mm} and} \\
				y_3 &= s(x_1 - x_3) - y_1 \Mod{p}
				\end{align*}				 								 
				where 
				\begin{equation*}
 				 s =\begin{cases}
				 \frac{y_2 - y_1}{x_2 - x_1} \Mod{p}, & \text{if $P \neq Q$}\\
				 \\
   				 \frac{3x_1 + b}{2y_1} \Mod{p}, & \text{if $P = Q$}
  				\end{cases}
				\end{equation*}
			\end{frame}
			
			
			\iffalse
			\begin{frame}
				\frametitle{Double-and-Add Algorithm for Point Multiplication}
				\textbf{How can we efficiently compute $dP$?} \\
				\pause
				\textbf{Step 1:} Write $d$ in binary, i.e. $$d = \sum\limits_{i = 0}^{t}d_i2^i$$
				where $d_i \in \lbrace 0, 1 \rbrace$ and $d_t = 1$ \\
				\pause
				\textbf{Step 2:} FOR $i = t-1$ DOWNTO 0
				\begin{center}
				$T = T + T \Mod{n}$ \\
				IF $d_i = 1$ \\
				\hspace{4mm} $T = T + P \Mod{n}$\\
				\end{center}
				\hspace{13mm} RETURN $(T)$
			\end{frame}
			
			\begin{frame}
			\frametitle{Double-and-Add Algorithm for Point Multiplication (cont)}
			\textbf{Example:}\\
			Supposed we want to quickly compute $26P$ \\
			Should I even make this slide? This seems a little more computer science-y \\
			It requires on average $1.5t$ point doubles and additions.
			\end{frame}
			\fi
			
			\begin{frame}
			\subsection{Hasse's Theorm}
			\frametitle{Cardinality}
			\textbf{Question:} What is the order of the group $(E, +) \Mod{p}$, i.e. how many point are on $E$? \\
			\  \\
			\  \\
			\pause
			\textbf{Hasse's Bound:} Given an elliptic curve $E$ modulo $p$, the number of points on $E$, 				denoted $\#E$, is bounded by
			$$p + 1 - 2\sqrt{p} \leq \#E \leq p + 1 + 2\sqrt{p}$$
			\  \\
			\  \\
			%maybe you can include the practical applications here
			
			\end{frame}
			
			\iffalse
			\begin{frame}
			\subsection{Elliptic Curves mod p}
			\frametitle{Elliptic Curves $\Mod {p}$}
			\textbf{The Discrete Logarithm Problem:}\\ 
			Given a prime $p$, and two nonzero elements $g$ and $b$ $\Mod{p}$, the discrete log problem is 			the problem of determining an integer $1 \leq k \leq p-1$ such that
			$$g^k = b \Mod {p}$$
			\pause
			\vspace{20mm}
			\  \\
			\textbf{Note:} Discrete logs are very hard to compute in general when $p$ is sufficiently 					large and is as difficult as factoring $\Mod{p}$
			%Make sure that this statement is true. Maybe give an example of current record for how large people have been able to get those integers
			\end{frame}
			\fi
			
			\begin{frame}
			\subsection{Discrete Log Problem for Elliptic Curves}
			\frametitle{Elliptic Curves $\Mod{p}$}
			\textbf{The Discrete Logarithm Problem for Elliptic Curves:}
			\  \\
			\  \\
			%Make it look like page 247 in Paar... you'll have to google it
			Given an elliptic curve $E$ and two points $A$ and $B$ on $E$, the discrete log problem for elliptic curves is finding an integer $1 \leq d \leq \#E$ such that
			$$\underbrace{P + P + \cdots + P}_\text{d times} = dP = T$$
			\  \\
			\  \\
			\  \\
			\pause
			In cryptosystems $d$ is the private key and $T$ is the public key
			
			\end{frame}
			
			\begin{frame}
			\subsection{Representing Plaintext}
			\frametitle{Representing Plaintext}
			We need a method for encoding a message as point on an elliptic curve.
			\  \\
			\  \\
			\pause
			\textbf{The Bad News:} Currently there is no known polynomial time, deterministic algorithm for writing points on an arbitrary elliptic curve. \\
			%deterministic means that the algorithm produces a function
			\  \\
			\pause
			\textbf{The Good News:} There are fast probabilistic methods for finding points\\
			\pause
			\  \\
			\textbf{With appropriately chosen parameters, the probability of failure can be made arbitrarily small.}\\ 
			%On the order of $1/2^{30}$   
			
			\end{frame}
			
			
			\begin{frame}
			\subsection{Representing Plaintext (cont)}
			\frametitle{Representing Plaintext}
			Let $E: y^2 \equiv x^3 + bx + c \Mod{p}$ be the elliptic curve and let $m$ be the message represented as a number. \\
			\  \\
			\  \\
			\pause
			\textbf{Idea:} Embed $m$ as the $x$-coordinate of a point on $E$ \\
			\pause
			\  \\
			\  \\
			\textbf{The Bad News:} There is only a 50\% chance that $m^3 + bm + c$ is a square modulo $p$
			\  \\
			\  \\
			\  \\
			\pause
			\textbf{Question:} How can we guarantee a higher success rate? \\
			\  \\
			\pause
			\textbf{Answer:} We'll adjoin a few bits at the end of $m$ and adjust them until we get a number $x$ such that $x^3 + bx + c$ is a square $\Mod{p}$
			\end{frame}
			
			
			\begin{frame}
			\subsection{Koblitz's Method}
			\frametitle{Koblitz's Method}
			Let $E: y^2 \equiv x^3 + bx + c \Mod{p}$ be the elliptic curve and let $m$ be the message represented as a number. \\
			\pause
			\begin{itemize}
			\item Let $K \in \mathbb{Z}$ be large enough such that a failure rate of $1/2^K$ is acceptable
			\pause
			\item Assume that $(m + 1)K < p$ and let $x = mK + j$
			\pause
			\item For $j = 0, 1, 2, \ldots, K - 1$,
			\pause
			\begin{enumerate}[-]
			\item Compute $x^3 + bx + c$ and try to calculate the square root $\Mod{p}$
			\pause
			\item If $x^3 + bx + c$ is a square, then we send $m$ to $P_m = (x, y)$, otherwise increment $j$ by 1
			\pause
			\item If we reach $j = K$, then we have failed to map a message to a point on $E$
			\end{enumerate}			
			\end{itemize}
			\end{frame}
			
			
			\begin{frame}
			\subsection{Decoding via Koblitz's Method}
			\frametitle{Decoding}
			\textbf{Note:} Because $x^3 + bx + c$ is a square approximately half of the time and we try 
			$x = mK + j$ at most $K$ times, we have about  $1/2^K$ chance of failure. \\
			\  \\
			\  \\
			\  \\
			\  \\
			\pause
			To recover the original message from $P_m = (x, y)$, we calculate\\
			$$ m = \floor{\frac{x}{K}}$$
			\  \\
			\  \\
			\textbf{Second Note:} Decoding requires that $(m + 1)K < p$
			\end{frame}
			
			
			\begin{frame}
			\subsection{ECDH}
			\frametitle{Elliptic Curve Diffie-Hellman Key Exchange (ECDH)}
			Suppose that Alice and Bob want to exchange a key\\
			\pause
			\begin{enumerate}
			\item They agree on a prime $p$, the elliptic curve $E: y^2 \equiv x^3 + ax + b \Mod{p}$,
			and a base point $P$ on $E$.
			\pause
			\item Alice randomly chooses an integer $k_a$ and Bob randomly chooses an integer $k_b$,
			which they keep secret
			\pause
			\item Alice publishes the point $A = k_aP$ and sends it to Bob
			\pause 
			\item Bob publishes the point $B = k_bP$ and sends it to Alice
			\pause
			\item Alice takes Bob's point $B$ and computes $k_a(B)$
			\pause
			\item Similarly, Bob computes $k_b(A)$
			\pause
			\item Because the group $(E, +)$ is abelian, 
			$$k_a(B) = k_a(k_bP) = k_b(k_aP) = k_b(A),$$
			so Alice and Bob have the same key
			\end{enumerate}
			\end{frame}
			
			
			\begin{frame}
			\subsection{El Gamal ECDSA}
			\frametitle{ElGamal Elliptic Curve Digital Signature Algorithm (ECDSA)}
			Suppose that Alice wants to sign a message, $m$, for Bob to verify.\\
			\pause
			To set up the system, we 
			\begin{enumerate}
			\item Fix an Elliptic Curve $E \Mod{p}$ where $p$ is large prime
			\pause
			\item Fix a base point $A$ on $E$
			\pause
			\item Assume that the message represented as a number $m$ satisfies
			$$0 \leq m \leq \#E$$
			\pause
			\item Alice chooses a private integer $a$ and computes $B = aA$
			\end{enumerate}
			\pause
			\  \\
			Now $(p, E, \#E, A, B)$ are made public while $a$ is private. 
			\end{frame}
			
			
			\begin{frame}
			\subsection{Signing a Message}
			\frametitle{El Gamal ECDSA: Signing a Message}
			Now Alice wants to sign the message, so she
			\pause
			\begin{enumerate}
			\item chooses a random $1 \leq k \leq \#E$ with gcd$(k, \#E) = 1$,
			\pause
			\item computes $kA \equiv R = (x, y)$,
			\pause
			\item computes $s \equiv k^{-1}(m-ax) \mod{\#E}$,
			\pause
			\item sends the signed message $(m, R, s)$ to Bob for verification,
			\end{enumerate}
			\end{frame}
			
			
			\begin{frame}
			\subsection{Verifying the Message}
			\frametitle{El Gamal ECDSA: Verifying a Message}
			To verify Alice's message, Bob
			\pause
			\begin{enumerate}
			\item downloads Alice's public info and $(p, E, \#E, A, B)$,
			\pause
			\item computes $v_1 \equiv xB + sR$ and $v_2 \equiv mA$
			\end{enumerate}	
			\  \\
			The signature is valid only if $v_1 = v_2$		 
			\end{frame}
			
			
			\begin{frame}
			\subsection{Why Does this work?}
			\frametitle{Why does this work?}
			We know that 
			\begin{align*}
			\action<+->{v_1 &= xB + sR} \\
			\action<+->{&= xaA + (k^{-1}(m - ax))(kA)} \\
			\action<+->{&= xaA + (m - ax)A} \\
			\action<+->{&= mA} \\
			\action<+->{&\equiv v_2} 
			\end{align*}
			\end{frame}
			
			
			\begin{frame}
			\subsection{Identity ECC: Intro}
			\frametitle{Identity-Based Encryption}
			\textit{In most public key systems, when Alice wants to send a message to Bob, she looks up his public key in a directory and then encrypts her message. However, how does she know that the information has not been modified by Eve and the public key listed for Bob is Eve's key?!}
			\pause
			\  \\
			\  \\
			\  \\
			\  \\
			Wouldn't it be nice to have a system where Bob's public identification information (like his email address) serves as the public key?			
			\end{frame}
			
			
			\begin{frame}
			\subsection{Weil Pairing}
			\frametitle{Setting up the Cryptosystem}
			First, let $p$ be a prime of the form $6q - 1$ where $q$ is also prime.\\
			Then for the elliptic curve $E: y^2 = x^3 + 1 \Mod{p}$, we know that\\
			\pause
			\begin{itemize}
			\item There is a point $P_0 \neq \infty$ such that $qP_0 = \infty$.
			\pause
			\item There is a function $\tilde{e}$ such that
			\begin{enumerate}[-]
			\item $\tilde{e}$ maps pairs of points $(aP_0, bP_0)$ to $q$th roots of unity
			\pause
			\item $\tilde{e}$ satisfies the \textbf{bilinearity property}
			$$\tilde{e}(aP_0, bP_0) = \tilde{e}(P_0, P_0)^{ab}$$
			for all $a$ and $b$
			\pause
			\item Given $P= kP_0$ and $Q= mP_0$, $\tilde{e}(P, Q)$ can be computed quickly from the coordinates $P$ and $Q$
			\pause
			\item $\tilde{e}(P_0, P_0) \neq 1$, so it is a nontrivial root of unity
			\end{enumerate}
			\end{itemize}
			\end{frame}
			
			
			\begin{frame}
			\subsection{The Two Hash Functions}
			\frametitle{Setting up the Cryptosystem (cont)}
			We need two public hash functions:
			\pause
			\begin{itemize}
			\item $H_1: \lbrace \text{arb. length binary string} \rbrace \longrightarrow kP_0$ \\ 
			for $k \in \mathbb{Z}$
			\  \\
			\item $H_2: \lbrace q \text{th root of unity} \rbrace \longrightarrow \lbrace \text{binary strings of length }  n \rbrace$ \\
			where $n$ is the length of the message to be sent
			\end{itemize}
			\end{frame}
			
			
			\begin{frame}
			\subsection{Trusted Authority}
			\frametitle{Setting up the System}
			To set up the system, we need a Trusted Authority, Arthur. Arthur does the following:
			\pause
			\begin{itemize}
			\item He chooses a secret integer $s$
			\pause
			\item He computes $P_1 = sP_0$, which is made public
			\pause
			\item For each User, Arthur finds the user's $ID$ (written as a binary string) and computes
			$$D_{User} = sH_1(ID),$$
			which is a point on $E$
			\pause
			\item Arthur sends $D_{User}$ to each user, who keeps it secret. He then discards $D_{User}$ 
			\end{itemize}
			\end{frame}
			
			
			%Be sure to write on the board what is public and what is private		
			
			
			\begin{frame}
			\subsection{Sending a Message}
			\frametitle{Sending a Message}
			Suppose Alice wants to send a message $m$ to Bob and suppose that $m$ is of binary length $n$.
			\  \\
			Bob's $ID$ is bob@computer.com, so Alice does the following:
			\pause
			\begin{enumerate}
			\item She computes $g \equiv \tilde{e}(H_1(bob@computer.com), P_1)$, a $q$th root of unity
			\pause
			\item She chooses a random integer $r \neq 0 \Mod{q}$ and computes
			$$t \equiv m \oplus H_2(g^r)$$
			where $\oplus$ is the XOR cipher.
			\pause
			\item She sends Bob the ciphertext
			$$c \equiv (rP_0, t),$$
			where $rP_0$ on $E$ and $t$ is a binary string of length $n$
			\end{enumerate}
			\end{frame}
			
			
			\begin{frame}
			\subsection{Decoding the message}
			\frametitle{Recovering the Message}
			Bob receives the pair $(U, v)$ where $U$ is a point on $E$ and $v$ is a binary string of length $n$. Then he does the following:
			\pause
			\begin{enumerate}
			\item He computes $h \equiv \tilde{e}(D_{Bob}, U)$, which is a $q$th root of unity
			\pause
			\item He recovers the message by
			$$m = v \oplus H_2(h)$$
			\end{enumerate}
			\end{frame}
			
			
			\begin{frame}
			\subsection{How it works}
			\frametitle{Why does this work?}
			If encryption is performed correction, $U = rP_0$ and $v = t = m \oplus H_2(g)$. \\
			\pause
			Since $D_{Bob} = sH_1(bob@computer.com)$,
			\begin{align*}
			h \equiv \tilde{e}(D_{Bob}, rP_0) &= \tilde{e}(sH_1(bob@computer.com), rP_0) \\
			\action<+->{&= \tilde{e}(H_1(bob@computer.com), P_0)^{rs}} \\
			\action<+->{&= \tilde{e}(H_1(bob@computer.com), sP_0)^{r}}\\
			\action<+->{&= \tilde{e}(H_1(bob@computer.com), P_1)^{r}} \\
			\action<+->{&\equiv g^r}
			\end{align*}
			$\action<+->{\text{Therefore,}}$
			$$\action<+->{t \oplus H_2(h) = t \oplus H_2(g^r) = (m \oplus H_2(g^r)) \oplus H_2(g^r) = m}$$	
			\end{frame}


			\begin{frame}
			\subsection{Any Questions?}
			\  \\
			\  \\
			\  \\
			\  \\
			\textbf{Any Questions?}
			\end{frame}
\end{document}
