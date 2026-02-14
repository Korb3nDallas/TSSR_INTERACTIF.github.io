
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ULTIMATE NET LAB — Bash, Powershell, Cisco & Subnetting</title>
    <style>
        :root {
            --bg: #0b1120;
            --panel: #1e293b;
            --text: #e2e8f0;
            --win-blue: #3b82f6;
            --lin-green: #22c55e;
            --cisco-orange: #f97316;
            --sub-purple: #a855f7;
            --font-mono: 'Consolas', 'Monaco', 'Courier New', monospace;
        }

        body { font-family: system-ui, sans-serif; background: var(--bg); color: var(--text); margin: 0; min-height: 100vh; display:flex; flex-direction:column; }
        
        /* Navigation */
        nav { background: #0f172a; padding: 1rem; border-bottom: 1px solid #334155; position: sticky; top:0; z-index:100; box-shadow: 0 4px 15px rgba(0,0,0,0.4); }
        .nav-inner { max-width: 1200px; margin: 0 auto; display: flex; justify-content: space-between; align-items: center; flex-wrap:wrap; gap:10px; }
        .brand { font-weight: 900; font-size: 1.4rem; background: linear-gradient(90deg, #3b82f6, #f97316); -webkit-background-clip: text; -webkit-text-fill-color: transparent; }
        
        .tabs { display: flex; gap: 8px; flex-wrap: wrap; }
        .tab { background: #1e293b; border: 1px solid #334155; color: #94a3b8; padding: 8px 14px; border-radius: 6px; cursor: pointer; transition: 0.2s; font-weight:600; font-size:0.9rem; }
        .tab:hover { background: #334155; color: white; }
        .tab.active { color: white; border-color: transparent; }
        
        .tab-win.active { background: var(--win-blue); box-shadow: 0 0 10px rgba(59, 130, 246, 0.4); }
        .tab-lin.active { background: var(--lin-green); box-shadow: 0 0 10px rgba(34, 197, 94, 0.4); }
        .tab-cis.active { background: var(--cisco-orange); box-shadow: 0 0 10px rgba(249, 115, 22, 0.4); }
        .tab-sub.active { background: var(--sub-purple); box-shadow: 0 0 10px rgba(168, 85, 247, 0.4); }

        /* Contenu */
        main { max-width: 1200px; margin: 2rem auto; padding: 0 1rem; flex:1; width:100%; box-sizing: border-box; }
        .view { display: none; animation: slideIn 0.3s ease-out; }
        .view.active { display: block; }
        @keyframes slideIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }

        h2 { border-left: 5px solid; padding-left: 15px; margin-bottom: 25px; display:flex; align-items:center; gap:10px; }

        /* TERMINALS UI */
        .term-container { display: grid; grid-template-columns: 250px 1fr; gap: 20px; align-items: start; }
        @media(max-width: 800px) { .term-container { grid-template-columns: 1fr; } }

        .cmd-list { display: flex; flex-direction: column; gap: 8px; max-height: 600px; overflow-y: auto; padding-right:5px; }
        .cmd-btn { 
            text-align: left; background: #1e293b; color: #cbd5e1; border: 1px solid #334155; 
            padding: 10px 12px; border-radius: 6px; cursor: pointer; font-family: var(--font-mono); font-size: 0.85rem; transition:0.2s;
            display: flex; justify-content: space-between; align-items: center;
        }
        .cmd-btn:hover { border-color: rgba(255,255,255,0.3); background: #283548; }
        .cmd-btn small { opacity: 0.6; font-family: sans-serif; font-size:0.75rem; }

        .console { 
            background: #0c0c0c; border-radius: 8px; border: 1px solid #333; 
            font-family: var(--font-mono); padding: 20px; min-height: 400px; max-height: 600px; overflow-y: auto;
            color: #ccc; position: relative; box-shadow: 0 10px 40px rgba(0,0,0,0.5);
        }
        .console-header { position: absolute; top:0; left:0; right:0; height: 30px; background: #1f1f1f; display: flex; gap:6px; padding: 8px; border-bottom:1px solid #333; }
        .dot { width:12px; height:12px; border-radius:50%; }
        .console-body { margin-top: 30px; white-space: pre-wrap; line-height: 1.5; }
        
        .prompt { font-weight: bold; }
        .win-prompt { color: #3b82f6; }
        .lin-prompt { color: #22c55e; }
        .cis-prompt { color: #f97316; }
        .cursor::after { content: '█'; animation: blink 1s infinite; margin-left:2px; }
        @keyframes blink { 50% { opacity: 0; } }

        /* SUBNETTING GAME */
        .subnet-board { background: #1e293b; padding: 30px; border-radius: 12px; border: 1px solid #4c1d95; box-shadow: 0 10px 30px rgba(0,0,0,0.3); text-align: center; }
        .ip-display { font-size: 2.5rem; font-family: var(--font-mono); color: #e879f9; margin: 20px 0; font-weight: bold; text-shadow: 0 0 15px rgba(232, 121, 249, 0.4); }
        .quiz-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(200px, 1fr)); gap: 20px; margin-top: 30px; }
        .input-group { text-align: left; }
        .input-group label { display: block; margin-bottom: 8px; color: #a5b4fc; font-size: 0.9rem; }
        .input-group input { width: 100%; padding: 12px; background: #0f172a; border: 2px solid #334155; border-radius: 6px; color: white; font-family: var(--font-mono); box-sizing: border-box; }
        .input-group input:focus { outline:none; border-color: #a855f7; }
        
        .btn-check { background: #a855f7; color: white; border: none; padding: 12px 30px; border-radius: 8px; font-weight: bold; font-size: 1rem; cursor: pointer; margin-top: 30px; transition: 0.2s; }
        .btn-check:hover { background: #9333ea; transform: scale(1.05); }

        .score-pill { background: #4c1d95; padding: 5px 12px; border-radius: 99px; font-size: 0.9rem; color: #e9d5ff; border: 1px solid #6b21a8; }
        
        .feedback { margin-top: 15px; min-height: 24px; font-weight: bold; }
        .correct { color: #4ade80; }
        .incorrect { color: #f87171; }

    </style>
</head>
<body>

<nav>
    <div class="nav-inner">
        <div class="brand">⚡ NetLab Ultimate</div>
        <div class="tabs">
            <button class="tab tab-win active" onclick="switchView('win')">Windows PowerShell</button>
            <button class="tab tab-lin" onclick="switchView('lin')">Linux Bash</button>
            <button class="tab tab-cis" onclick="switchView('cis')">Cisco IOS</button>
            <button class="tab tab-sub" onclick="switchView('sub')">Subnetting Game</button>
        </div>
    </div>
</nav>

<main>
    <div id="win" class="view active">
        <h2 style="color:var(--win-blue)">Windows PowerShell & CMD</h2>
        <div class="term-container">
            <div class="cmd-list" id="winCmds"></div>
            <div class="console">
                <div class="console-header">
                    <div class="dot" style="background:#ff5f56"></div><div class="dot" style="background:#ffbd2e"></div><div class="dot" style="background:#27c93f"></div>
                    <span style="margin-left:10px; font-size:11px; color:#888">Administrator: Windows PowerShell</span>
                </div>
                <div class="console-body" id="winOut">
                    <div style="margin-bottom:10px">Windows PowerShell<br>Copyright (C) Microsoft Corporation. All rights reserved.</div>
                    <div class="line"><span class="prompt win-prompt">PS C:\Users\Admin></span> <span class="cursor"></span></div>
                </div>
            </div>
        </div>
    </div>

    <div id="lin" class="view">
        <h2 style="color:var(--lin-green)">Linux Bash (Debian/Kali)</h2>
        <div class="term-container">
            <div class="cmd-list" id="linCmds"></div>
            <div class="console">
                <div class="console-header">
                    <div class="dot" style="background:#ff5f56"></div><div class="dot" style="background:#ffbd2e"></div><div class="dot" style="background:#27c93f"></div>
                    <span style="margin-left:10px; font-size:11px; color:#888">root@kali:~</span>
                </div>
                <div class="console-body" id="linOut">
                    <div class="line"><span class="prompt lin-prompt">root@kali:~#</span> <span class="cursor"></span></div>
                </div>
            </div>
        </div>
    </div>

    <div id="cis" class="view">
        <h2 style="color:var(--cisco-orange)">Cisco IOS - Packet Tracer Mode</h2>
        <p style="color:#94a3b8; margin-top:-20px; margin-bottom:20px; font-size:0.9rem;">
            Clique sur les commandes pour configurer le routeur. Observe le changement de prompt (User > Privileged > Config).
        </p>
        <div class="term-container">
            <div class="cmd-list" id="cisCmds"></div>
            <div class="console" style="background:#1e1e1e; font-family:'Courier New'; font-weight:bold;">
                <div class="console-header">
                    <div class="dot" style="background:#ff5f56"></div>
                    <span style="margin-left:10px; font-size:11px; color:#888">Router0 - CLI</span>
                </div>
                <div class="console-body" id="cisOut" style="color:#f97316;">
                    <div>System Bootstrap, Version 15.1(4)M4, RELEASE SOFTWARE (fc1)</div>
                    <div>Press RETURN to get started!</div>
                    <br>
                    <div class="line"><span class="prompt cis-prompt">Router></span> <span class="cursor"></span></div>
                </div>
            </div>
        </div>
    </div>

    <div id="sub" class="view">
        <div style="display:flex; justify-content:space-between; align-items:center;">
            <h2 style="color:var(--sub-purple)">Subnetting Challenge</h2>
            <div class="score-pill">Exercices réussis : <span id="scoreVal">0</span> / 20</div>
        </div>
        
        <div class="subnet-board">
            <p style="color:#94a3b8;">Calcule les informations pour le réseau suivant :</p>
            <div class="ip-display" id="qIP">192.168.1.10 / 24</div>
            
            <div class="feedback" id="feedback"></div>

            <div class="quiz-grid">
                <div class="input-group">
                    <label>Adresse Réseau (Network ID)</label>
                    <input type="text" id="ansNet" placeholder="ex: 192.168.1.0">
                </div>
                <div class="input-group">
                    <label>Première IP Utilisable</label>
                    <input type="text" id="ansFirst" placeholder="ex: 192.168.1.1">
                </div>
                <div class="input-group">
                    <label>Dernière IP Utilisable</label>
                    <input type="text" id="ansLast" placeholder="ex: 192.168.1.254">
                </div>
                <div class="input-group">
                    <label>Adresse de Diffusion (Broadcast)</label>
                    <input type="text" id="ansBroad" placeholder="ex: 192.168.1.255">
                </div>
            </div>

            <button class="btn-check" onclick="checkSubnet()">Vérifier & Suivant</button>
            <button onclick="nextSubnet()" style="background:transparent; border:none; color:#94a3b8; text-decoration:underline; cursor:pointer; margin-top:10px; font-size:0.9rem;">Passer cette question</button>
        </div>
    </div>
</main>

<script>
/* =========================================
   DATA - COMMANDES
   ========================================= */

const WIN_CMDS = [
    { c: "ipconfig /all", d: "Détails complets IP (MAC, DNS)", r: "Configuration IP de Windows\n\nCarte Ethernet Ethernet0 :\n   Suffixe DNS propre à la connexion. . . : localdomain\n   Adresse IPv6. . . . . . . . . . . . . .: fe80::a1b2:c3d4%12\n   Adresse IPv4. . . . . . . . . . . . . .: 192.168.1.105\n   Masque de sous-réseau. . . . . . . . . : 255.255.255.0\n   Passerelle par défaut. . . . . . . . . : 192.168.1.1\n   Adresse physique . . . . . . . . . . . : 00-0C-29-3E-4F-5A" },
    { c: "ping 8.8.8.8", d: "Test connectivité", r: "Envoi d'une requête 'Ping'  8.8.8.8 avec 32 octets de données :\nRéponse de 8.8.8.8 : octets=32 temps=14 ms TTL=115\nRéponse de 8.8.8.8 : octets=32 temps=12 ms TTL=115\nRéponse de 8.8.8.8 : octets=32 temps=15 ms TTL=115\n\nStatistiques Ping pour 8.8.8.8:\n    Paquets : envoyés = 3, reçus = 3, perdus = 0 (perte 0%)" },
    { c: "tracert google.com", d: "Trace la route (routeurs)", r: "Détermination de l'itinéraire vers google.com [142.250.75.238]\n  1    <1 ms    192.168.1.1\n  2    15 ms    80.12.34.1 (Box FAI)\n  3    20 ms    193.252.1.1 (Backbone)\n  4    22 ms    142.250.75.238\n\nItinéraire déterminé." },
    { c: "netstat -an", d: "Ports ouverts & Connexions", r: "Connexions actives\n\n  Proto  Adresse locale         Adresse distante       État\n  TCP    0.0.0.0:135            0.0.0.0:0              LISTENING\n  TCP    0.0.0.0:445            0.0.0.0:0              LISTENING\n  TCP    192.168.1.105:50432    20.198.118.190:443     ESTABLISHED\n  UDP    0.0.0.0:53             *:* " },
    { c: "Get-NetIPAddress", d: "PowerShell: Infos IP", r: "IPAddress         : 192.168.1.105\nInterfaceIndex    : 12\nInterfaceAlias    : Ethernet0\nAddressFamily     : IPv4\nType              : Unicast\nPrefixLength      : 24\nPrefixOrigin      : Dhcp" },
    { c: "Test-NetConnection google.com", d: "PowerShell: Test Port/Ping", r: "ComputerName           : google.com\nRemoteAddress          : 142.250.75.238\nInterfaceAlias         : Ethernet0\nSourceAddress          : 192.168.1.105\nPingSucceeded          : True\nPingReplyDetails (RTT) : 14 ms" },
    { c: "Resolve-DnsName google.com", d: "PowerShell: DNS Lookup", r: "Name                                           Type   TTL   Section    IPAddress\n----                                           ----   ---   -------    ---------\ngoogle.com                                     AAAA   294   Answer     2607:f8b0:4005:809::200e\ngoogle.com                                     A      294   Answer     142.250.75.238" },
    { c: "arp -a", d: "Table ARP (IP <-> MAC)", r: "Interface : 192.168.1.105 --- 0xc\n  Adresse Internet      Adresse physique      Type\n  192.168.1.1           a4-3e-5b-12-34-56     dynamique\n  192.168.1.255         ff-ff-ff-ff-ff-ff     statique" },
    { c: "nslookup microsoft.com", d: "Requête DNS simple", r: "Serveur :  UnKnown\nAddress:  192.168.1.1\n\nRéponse ne faisant pas autorité :\nNom :    microsoft.com\nAddresses:  20.103.85.33\n          20.81.111.85" }
];

const LIN_CMDS = [
    { c: "ip a", d: "Affiche les interfaces IP", r: "1: lo: <LOOPBACK,UP> mtu 65536 qdisc noqueue state UNKNOWN\n    inet 127.0.0.1/8 scope host lo\n2: eth0: <BROADCAST,MULTICAST,UP> mtu 1500 state UP\n    link/ether 00:0c:29:3e:4f:5a brd ff:ff:ff:ff:ff:ff\n    inet 192.168.1.20/24 brd 192.168.1.255 scope global eth0" },
    { c: "ping -c 3 1.1.1.1", d: "Ping (3 paquets)", r: "PING 1.1.1.1 (1.1.1.1) 56(84) bytes of data.\n64 bytes from 1.1.1.1: icmp_seq=1 ttl=58 time=12.4 ms\n64 bytes from 1.1.1.1: icmp_seq=2 ttl=58 time=11.8 ms\n64 bytes from 1.1.1.1: icmp_seq=3 ttl=58 time=12.1 ms\n\n--- 1.1.1.1 ping statistics ---\n3 packets transmitted, 3 received, 0% packet loss" },
    { c: "ss -tulpn", d: "Ports en écoute (remplace netstat)", r: "Netid  State   Recv-Q  Send-Q   Local Address:Port   Peer Address:Port  Process\nudp    UNCONN  0       0        0.0.0.0:68           0.0.0.0:* users:((\"dhclient\",pid=542,fd=6))\ntcp    LISTEN  0       128      0.0.0.0:22           0.0.0.0:* users:((\"sshd\",pid=678,fd=3))\ntcp    LISTEN  0       100      127.0.0.1:25         0.0.0.0:* users:((\"master\",pid=890,fd=13))" },
    { c: "dig google.com", d: "DNS Lookup complet", r: ";; QUESTION SECTION:\n;google.com.			IN	A\n\n;; ANSWER SECTION:\ngoogle.com.		196	IN	A	142.250.179.110\n\n;; SERVER: 192.168.1.1#53(192.168.1.1)\n;; MSG SIZE  rcvd: 55" },
    { c: "ip route", d: "Table de routage", r: "default via 192.168.1.1 dev eth0 proto dhcp src 192.168.1.20 metric 100\n192.168.1.0/24 dev eth0 proto kernel scope link src 192.168.1.20" },
    { c: "cat /etc/resolv.conf", d: "Voir configuration DNS", r: "# Generated by NetworkManager\nnameserver 192.168.1.1\nnameserver 8.8.8.8" },
    { c: "mtr -rwc 1 1.1.1.1", d: "My Traceroute (Ping+Trace)", r: "HOST: kali-linux              Loss%   Snt   Last   Avg  Best  Wrst StDev\n  1.|-- 192.168.1.1           0.0%     1    0.5   0.5   0.5   0.5   0.0\n  2.|-- 80.12.34.1            0.0%     1   15.2  15.2  15.2  15.2   0.0\n  3.|-- one.one.one.one       0.0%     1   13.4  13.4  13.4  13.4   0.0" },
    { c: "sudo tcpdump -i eth0 -c 5", d: "Sniffer le trafic (Shark)", r: "tcpdump: verbose output suppressed, use -v or -vv for full protocol decode\nlistening on eth0, link-type EN10MB (Ethernet), capture size 262144 bytes\n14:22:01.45 IP 192.168.1.20.22 > 192.168.1.10.55443: Flags [P.], seq 1:44, ack 1, win 502\n14:22:01.46 IP 192.168.1.10.55443 > 192.168.1.20.22: Flags [.], ack 44, win 2048\n5 packets captured" }
];

const CISCO_CMDS = [
    { c: "enable", d: "Passe en mode Privilégié (#)", r: "", newPrompt: "Router#" },
    { c: "show ip interface brief", d: "Résumé des interfaces", r: "Interface              IP-Address      OK? Method Status                Protocol\nGigabitEthernet0/0     unassigned      YES unset  administratively down down\nGigabitEthernet0/1     unassigned      YES unset  administratively down down\nVlan1                  unassigned      YES unset  administratively down down", requiredPrompt: "Router#" },
    { c: "configure terminal", d: "Mode Configuration Globale", r: "Enter configuration commands, one per line.  End with CNTL/Z.", newPrompt: "Router(config)#", requiredPrompt: "Router#" },
    { c: "hostname R1", d: "Changer le nom du routeur", r: "", newPrompt: "R1(config)#", requiredPrompt: "Router(config)#" },
    { c: "interface g0/0", d: "Configurer l'interface G0/0", r: "", newPrompt: "R1(config-if)#", requiredPrompt: "R1(config)#" },
    { c: "ip address 192.168.1.1 255.255.255.0", d: "Assigner une IP", r: "", requiredPrompt: "R1(config-if)#" },
    { c: "no shutdown", d: "Activer l'interface", r: "%LINK-5-CHANGED: Interface GigabitEthernet0/0, changed state to up\n%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0, changed state to up", requiredPrompt: "R1(config-if)#" },
    { c: "end", d: "Retour au mode privilégié", r: "%SYS-5-CONFIG_I: Configured from console by console", newPrompt: "R1#", requiredPrompt: "R1(config-if)#" },
    { c: "show ip route", d: "Voir la table de routage", r: "Gateway of last resort is not set\n\n      192.168.1.0/24 is variably subnetted, 2 subnets, 2 masks\nC        192.168.1.0/24 is directly connected, GigabitEthernet0/0\nL        192.168.1.1/32 is directly connected, GigabitEthernet0/0", requiredPrompt: "R1#" },
    { c: "write memory", d: "Sauvegarder la config", r: "Building configuration...\n[OK]", requiredPrompt: "R1#" }
];

/* =========================================
   LOGIQUE TERMINAL
   ========================================= */

let currentPrompts = {
    win: "PS C:\\Users\\Admin>",
    lin: "root@kali:~#",
    cis: "Router>"
};

function initTerminals() {
    renderCmds('win', WIN_CMDS);
    renderCmds('lin', LIN_CMDS);
    renderCmds('cis', CISCO_CMDS);
}

function renderCmds(type, list) {
    const container = document.getElementById(type + 'Cmds');
    list.forEach(item => {
        const btn = document.createElement('div');
        btn.className = 'cmd-btn';
        btn.innerHTML = `<span>> ${item.c}</span><small>${item.d}</small>`;
        btn.onclick = () => runCmd(type, item);
        container.appendChild(btn);
    });
}

function runCmd(type, item) {
    const out = document.getElementById(type + 'Out');
    const promptClass = type + '-prompt';
    
    // Cisco Logic: Check modes
    if (type === 'cis') {
        const currentP = currentPrompts.cis;
        if (item.requiredPrompt && !currentP.includes(item.requiredPrompt.replace('#','').replace('>',''))) {
            // Check loose match (ex: R1# matches Router# requirement logic handled simply here)
            // Simplification: check if ends with correct symbol
            const reqSymbol = item.requiredPrompt.slice(-1);
            const currSymbol = currentP.slice(-1);
            
            // Allow if both are config modes or both are exec
            const isConfig = currentP.includes('(config');
            const reqConfig = item.requiredPrompt.includes('(config');
            
            if (reqSymbol !== currSymbol || (isConfig !== reqConfig && item.c !== 'end')) {
                typeEffect(out, currentP, item.c, "% Invalid input detected at marker (Wrong Mode).", promptClass);
                return;
            }
        }
    }

    // Remove old cursor
    const oldCursor = out.querySelector('.cursor');
    if(oldCursor) oldCursor.remove();

    // Add command line
    const cmdLine = document.createElement('div');
    cmdLine.innerHTML = `<span class="prompt ${promptClass}">${currentPrompts[type]}</span> ${item.c}`;
    out.appendChild(cmdLine);

    // Update Prompt if needed (Cisco)
    if (item.newPrompt) currentPrompts[type] = item.newPrompt;

    // Simulate Output
    setTimeout(() => {
        if (item.r) {
            const respDiv = document.createElement('div');
            respDiv.style.whiteSpace = "pre-wrap";
            respDiv.style.color = type === 'cis' ? '#e2e8f0' : 'inherit'; // White text for cisco output
            respDiv.textContent = item.r;
            out.appendChild(respDiv);
        }
        
        // Add new prompt line
        const newPromptLine = document.createElement('div');
        newPromptLine.className = "line";
        newPromptLine.innerHTML = `<span class="prompt ${promptClass}">${currentPrompts[type]}</span> <span class="cursor"></span>`;
        out.appendChild(newPromptLine);
        
        out.scrollTop = out.scrollHeight;
    }, 300);
}

function typeEffect(container, prompt, cmd, response, pClass) {
    // Helper simple sans animation de frappe pour fluidité, mais structure prête
    const div = document.createElement('div');
    div.innerHTML = `<span class="${pClass}">${prompt}</span> ${cmd}`;
    container.appendChild(div);
    const rDiv = document.createElement('div');
    rDiv.textContent = response;
    container.appendChild(rDiv);
    container.scrollTop = container.scrollHeight;
}

/* =========================================
   LOGIQUE SUBNETTING
   ========================================= */

let currentSubnet = {};
let score = 0;

function ipToInt(ip) {
    return ip.split('.').reduce((acc, octet) => (acc << 8) + parseInt(octet, 10), 0) >>> 0;
}

function intToIp(int) {
    return [
        (int >>> 24) & 255,
        (int >>> 16) & 255,
        (int >>> 8) & 255,
        int & 255
    ].join('.');
}

function generateSubnet() {
    // Generate Random /24 to /30
    const cidr = Math.floor(Math.random() * (30 - 20) + 20); // /20 to /30
    
    // Random IP
    const ipInt = Math.floor(Math.random() * 0xFFFFFFFF) >>> 0;
    
    // Calculate Data
    const mask = (0xFFFFFFFF << (32 - cidr)) >>> 0;
    const netAddr = (ipInt & mask) >>> 0;
    const broadcast = (netAddr | (~mask)) >>> 0;
    const first = (netAddr + 1) >>> 0;
    const last = (broadcast - 1) >>> 0;

    // Format for User
    // We give a random IP inside the subnet, user must find the Network ID
    const randomHost = Math.floor(Math.random() * (last - first + 1)) + first;
    const displayIP = intToIp(randomHost);

    currentSubnet = {
        cidr: cidr,
        display: `${displayIP} /${cidr}`,
        net: intToIp(netAddr),
        first: intToIp(first),
        last: intToIp(last),
        broad: intToIp(broadcast)
    };

    document.getElementById('qIP').textContent = currentSubnet.display;
    document.getElementById('feedback').textContent = "";
    
    // Reset Inputs
    ['ansNet', 'ansFirst', 'ansLast', 'ansBroad'].forEach(id => {
        const el = document.getElementById(id);
        el.value = "";
        el.style.borderColor = "#334155";
    });
}

function checkSubnet() {
    const getVal = id => document.getElementById(id).value.trim();
    
    const uNet = getVal('ansNet');
    const uFirst = getVal('ansFirst');
    const uLast = getVal('ansLast');
    const uBroad = getVal('ansBroad');
    
    let isCorrect = true;
    
    function validate(id, expected, user) {
        const el = document.getElementById(id);
        if(user === expected) {
            el.style.borderColor = "#4ade80"; // Green
        } else {
            el.style.borderColor = "#f87171"; // Red
            isCorrect = false;
        }
    }

    validate('ansNet', currentSubnet.net, uNet);
    validate('ansFirst', currentSubnet.first, uFirst);
    validate('ansLast', currentSubnet.last, uLast);
    validate('ansBroad', currentSubnet.broad, uBroad);

    const fb = document.getElementById('feedback');
    if(isCorrect) {
        fb.textContent = "✅ Correct ! Excellent travail.";
        fb.className = "feedback correct";
        if (score < 20) score++;
        document.getElementById('scoreVal').textContent = score;
        setTimeout(nextSubnet, 1500);
    } else {
        fb.innerHTML = `❌ Erreur. <br>Solutions: Réseau=${currentSubnet.net}, 1ère=${currentSubnet.first}, Der=${currentSubnet.last}, Broad=${currentSubnet.broad}`;
        fb.className = "feedback incorrect";
    }
}

function nextSubnet() {
    generateSubnet();
}


/* =========================================
   UI & INIT
   ========================================= */

function switchView(id) {
    document.querySelectorAll('.view').forEach(el => el.classList.remove('active'));
    document.getElementById(id).classList.add('active');
    
    document.querySelectorAll('.tab').forEach(el => el.classList.remove('active'));
    document.querySelector(`.tab-${id}`).classList.add('active');
}

// Start
initTerminals();
generateSubnet();

</script>
</body>
</html>
