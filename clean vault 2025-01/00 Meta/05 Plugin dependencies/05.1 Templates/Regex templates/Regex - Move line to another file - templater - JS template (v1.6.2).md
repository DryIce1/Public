<%*
//v1.6.2: Fix with update to Templater where wasn't removing the selected text/line "on move"

//'first' will add to top of file. 'last' will add to bottom of file
let firstOrLastLine = 'last';

//Choose a specific line to move to underneath; overrules firstOrLastLine if true
const bChooseLine = true;

//After moving the line, open the file it was moved to
const bOpenFile = false;

//"move" = default, "copy" = duplicate, "embed" = move and leave a block ref link, "blockRef" = create block ref and add an embed link in other file
const moveMode = "move";

const moveToFile = await tp.system.suggester((item) => item.path, this.app.vault.getMarkdownFiles(), false);
if(moveToFile) {
    let cmEditorAct = this.app.workspace.activeLeaf.view.editor;
    if(tp.file.selection() == '') {
        const curLine = cmEditorAct.getCursor().line;
        cmEditorAct.setSelection({ line: curLine, ch: 0 }, { line: curLine, ch: 9999 });
    }
    let selectedText = tp.file.selection();
    tR = selectedText;
    const curContent = await this.app.vault.read(moveToFile);
    let newContents;
    let selectLine;
    if(moveMode == "blockRef" || moveMode == "embed") {
        function createBlockHash() {
            let result = '';
            var characters = 'abcdefghijklmnopqrstuvwxyz0123456789';
            var charactersLength = characters.length;
            for ( var i = 0; i < 7; i++ ) {
                result += characters.charAt(Math.floor(Math.random() * charactersLength));
            }
            return result;
        }
        let id = createBlockHash();
        let blockRef;
        if(moveMode == "blockRef") {
            blockRef = `![[${tp.file.title}#^${id}]]`.split("\n").join("");
            tR = tp.file.selection() + ` ^${id}`.split("\n").join("");
            selectedText = blockRef;
        } else {
            blockRef = `![[${moveToFile.basename}#^${id}]]`.split("\n").join("");
            tR = blockRef;
            selectedText = '\n' + tp.file.selection() + ` ^${id}`.split("\n").join("") + '\n';
        }
        navigator.clipboard.writeText(blockRef).then(text => text);
    }
    if(bChooseLine) {
        const arrayEachLine = curContent.split('\n');
        let arrayStrings = ['--End of File--', '--Beginning of File--'];
        let arrayValues = ['last_', 'first_'];
        arrayEachLine.forEach(eachItem => {
            if(eachItem != '') {
                arrayStrings.push(eachItem.slice(0,250));
                arrayValues.push(eachItem);
            }
        });
        selectLine = await tp.system.suggester(arrayStrings, arrayValues, false);
        if(selectLine == 'last_' || selectLine == 'first_') {
            if(selectLine == 'first_'){
                firstOrLastLine = 'first';
                newContents = selectedText + '\n' + curContent;
            } else {
                firstOrLastLine = 'last';
                newContents = curContent + '\n' + selectedText;
            }
        } else {
            if(curContent.contains('\n' + selectLine + '\n')) {
                newContents = curContent.replace('\n' + selectLine + '\n', '\n' + selectLine + '\n' + selectedText + '\n');
            } else if(curContent.startsWith(selectLine + '\n')) {
                newContents = curContent.replace(selectLine + '\n', selectLine + '\n' + selectedText + '\n');
            } else {
                firstOrLastLine = 'last';
                newContents = curContent + '\n' + selectedText;
            }
        }
    } else {
        if(firstOrLastLine == 'first'){
            newContents = selectedText + '\n' + curContent
        }
        else {
            newContents = curContent + '\n' + selectedText
        }
    }
    if(bChooseLine == false || selectLine) {
        await this.app.vault.modify(moveToFile, newContents);
        if(bOpenFile) {
            await this.app.workspace.openLinkText(moveToFile.basename, moveToFile.path, true);
            if(bChooseLine == false || selectLine == 'last_' || selectLine == 'first_') {
                cmEditorAct = this.app.workspace.activeLeaf.view.editor;
                if(firstOrLastLine == 'first'){
                    cmEditorAct.setSelection({ line: 0, ch: 0 }, { line: 0, ch: 9999 });
                }
                else {
                    cmEditorAct.setSelection({ line: cmEditorAct.lastLine(), ch: 0 }, { line: cmEditorAct.lastLine(), ch: 9999 });
                }
            }
        }
        if(moveMode == "move") {
            cmEditorAct.replaceSelection('');
            tR = '';
        } else if(moveMode == "copy") {
            tR = selectedText;
        }
    }
}
%>