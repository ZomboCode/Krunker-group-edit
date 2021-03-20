// ==UserScript==
// @name         Built-in specific property editor
// @namespace    http://tampermonkey.net/
// @version      0.1
// @description  For working with massive trigger circuits
// @author       ZomboCom
// @match        https://krunker.io/editor.html
// @grant        none
// ==/UserScript==

function addGlobalStyle(css) {
    var head, style;
    head = document.getElementsByTagName('head')[0];
    if (!head) { return; }
    style = document.createElement('style');
    style.type = 'text/css';
    style.innerHTML = css.replace(/;/g, ' !important;');
    head.appendChild(style);
}

addGlobalStyle(`
#zombole {
    position:absolute;
    left:7%;
    top:0%;
    color:#ddd;
    width:1000%;
    height:1000%;
    border:none;
    background-color:rgba(0, 0, 0, 0);
    overflow:hidden;
    white-space: nowrap;
    font-family: Monaco, Menlo, Consolas, "Courier New", monospace;
    font-size:16px;
    -moz-tab-size:4;
    tab-size:4;
    white-space: pre-wrap;

}
#guide {
    padding-left:100px;
    width:calc(100% - 140px);
    height:20%;
    color:#ddd;
}
#sidenumbers {
    background-color:#555;
    position:absolute;
    overflow:hidden;
    left:0px;
    top:0px;
    width:5%;
    height:1000%;
    text-align:right;
    padding-right:3px;
    font-size:16px;
    font-family:Monaco, Menlo, Consolas, "Courier New", monospace;
}
#ide {
    position:absolute;
    top:25%;
    left:0%;
    overflow:scroll;
    width:100%;
    height:75%;
}
.closeButton {
    position:absolute;
    top:calc(0% - 10px);
    right:calc(0% - 10px);
    font-size:27px;
    font-family: Monaco, Menlo, Consolas, "Courier New", monospace;
    color:#fff;
    background-color:#555;
    border:10px solid black;
    padding:0px;
    width:50px;
    height:50px;
    text-align:top;
    border-radius:0 0 0 8px;

}
#runner {
    top:5px;
    left:10px;
    border-width:5px;
    width:100px;
    height:40px;
    padding:1px;
    font-size:20px;
    border-radius:8px;

}
#ondoer {
    top:55px;
    left:10px;
    border-width:5px;
    width:100px;
    height:40px;
    padding:1px;
    font-size:20px;
    border-radius:8px;
}
button:active {
    transform:translateY(4px);
}
button:hover{
    background-color:#777;
}
#openButton {
    position:absolute;
    z-index:2;
    bottom:0px;
    left:0px;
    width:60px;
    height:60px;
    background-color:#555;
    border:5px solid black;
    border-radius:8px;
    display:none;

}
#openArrow {
    padding-top:0px;
    position:absolute;
    bottom:-57px;
    left:10px;
    color:#fff;
    font-size:50px;

}



`);

function zomboEdit(e, eval1, eval2) {

    var n = KE.objectSelected(!0);
    if (!n) {
        return("No group selected");
    }
    var a = KE.objGroups[n.uuid];
    var rfgh = KE.objInsts.filter(e=>a.objects.includes(e.boundingMesh.uuid));
    eval(eval1);
    console.log(i);
    var saveData = {};
    for (var dfgh in rfgh) {
        var o = rfgh[dfgh];
        saveData[dfgh] = JSON.parse(JSON.stringify(o));
        eval(eval2);
    }
    console.log(saveData);
}
function mod() {
    var openButton = document.createElement("button");
    openButton.id = "openButton";
    openButton.innerHTML = "<p id = openArrow>&#8599</p>"
    document.body.appendChild(openButton);
    var cWrap = document.createElement('div');
    document.body.appendChild(cWrap);
    cWrap.innerHTML = `
<div id = guide>Press the run button to execute changes on every object in your selected group.</div>
<button id = closer class = closeButton>X</button>
<button id = runner class = closeButton>► Run</button>
<button id = ondoer class = closeButton>&larr; Undo</button>
<div id = ide>
<div id = "sidenumbers"></div>
<textarea id = zombole spellcheck = "false">
//This function gets executed before editing
//Use it for declaring variables
function start() {
    var i = 0
}

//This function gets executed on every object
//Use it for editing objects with o.<key> = <value>
//Keys are same as the ones in group edit
function modify() {
	i += 1
    //Red gradient
    o.color = "#" + Math.min(15, i*2).toString(16) + Math.min(15, i*2).toString(16) + "0000"
	//Growing ids
    o.interfaceId = i
    //Making objects destructible and giving them rising respawn timers
    o.canDestroy = true
	o.respawnT = i*0.5
}







</textarea>
</div>
`;
    openButton.addEventListener("click", function() {
        openButton.style.setProperty("display", "none", "important")
        cWrap.style.display = "inline";
    });
    document.getElementById("runner").addEventListener("click", function() {
        var started = false;
        var counter = 0;
        var start_id = null;
        var end_id = null;
        for (var i = 0; i < zombole.value.length; i++) {
            var char = zombole.value[i];
            if (char === "{") {
                counter += 1;
                if (!started) {
                    start_id = i;
                }
                started = true;
            }
            else if (char === "}") {
                counter -= 1;
            }
            if (counter === 0 && started) {
                end_id = i;
                break;
            }
        }
        var start = zombole.value.slice(start_id + 1, end_id);
        started = false;
        counter = 0;
        start_id = null;
        end_id = null;
        i+= 1
        for (i; i < zombole.value.length; i++) {
            char = zombole.value[i];
            if (char === "{") {
                counter += 1;
                if (!started) {
                    start_id = i;
                }
                started = true;
            }
            else if (char === "}") {
                counter -= 1;
            }
            if (counter === 0 && started) {
                end_id = i;
                break;
            }
        }
        console.log(start_id, end_id);
        var modify = zombole.value.slice(start_id + 1, end_id);
        if (start_id === null) {
            alert("Error. Can't find function modify()");
        }
        else if (end_id === null) {
            alert("Error. Can't find '}' to end function with");
        }
        else if (KE.copyGroup() === undefined) {
            alert("No group selected")
        }
        else {
            zomboEdit("interfaceId", start, modify);
        }
    });
    cWrap.id = "cWrap";
    var zombole = document.getElementById("zombole");
    document.getElementById("closer").addEventListener("click", function() {
        cWrap.style.display = "none";
        openButton.style.setProperty("display", "inline", "important")
        console.log("ding dong");
    })
    zombole.addEventListener("scroll", function() {
        cWrap.scrollTop = zombole.scrollTop;
        zombole.scrollTop = 0;
        console.log(zombole.scrollLeft)
    });
    //zombole.value = code;
    for (var i = 0; i < 100; i++) {
        document.getElementById("sidenumbers").innerHTML = document.getElementById("sidenumbers").innerHTML + i + "<br>";
    }
    cWrap.style.backgroundColor = "#242424";
    cWrap.style.width = "720px";
    cWrap.style.height = "480px";
    cWrap.style.display = "inline";
    cWrap.style.position = "absolute";
    cWrap.style.bottom = "0px";
    cWrap.style.left = "0px";
    cWrap.style.padding = "10px";
    cWrap.style.paddingLeft = "30px";
    cWrap.style.border = "10px solid black"
    cWrap.style.resize = "both";
    document.getElementById('zombole').addEventListener('keydown', function(e) {
        if (e.key == 'Tab') {
            e.preventDefault();
            var start = this.selectionStart;
            var end = this.selectionEnd;

            // set textarea value to: text before caret + tab + text after caret
            this.value = this.value.substring(0, start) +
                "\t" + this.value.substring(end);
            console.log("h");

            // put caret at right position again
            this.selectionStart =
                this.selectionEnd = start + 1;
        }

    });


};
