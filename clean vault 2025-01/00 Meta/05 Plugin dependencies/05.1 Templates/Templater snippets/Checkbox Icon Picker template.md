<%*  
// Define the checkbox options  
const slrvbCheckboxes = [  
    ["Unchecked", "Regular", "Checked", "Dropped", "Forward", "Defer", "Question", "Half Done", "Add", "Research", "Important", "Idea", "Brainstorm", "Pro", "Con", "Quote", "Note", "Bookmark", "Information"],  
    [" ", "x", "X", "-", ">", "D", "?", "/", "+", "R", "!", "i", "B", "P", "C", "Q", "N", "b", "I"]  
];

const sanctumCheckboxes = [  
    ["Unchecked", "Star", "Alarm / Reminder / Notification", "Favourite", "Savings / Piggy bank", "Savings alternative / Piggy bank", "Cancelled", "Rescheduled / Forwarded", "Scheduled", "Location", "Bug / Debug", "Failure", "Note / Annotation", "Pros", "Cons", "Win / Success / Reward", "Bookmark / References", "Idea / Tip", "Important / Attention", "Question / Cue", "Info"],  
    [" ", "⭐", "a", "❤", "s", "S", "-", ">", "<", "l", "B", "X", "n", "p", "c", "W", "b", "I", "!", "?", "i"]  
];

// Step 1: Let the user choose which checkbox set to use  
const checkboxSets = {
    "SLRVB": slrvbCheckboxes,
    "Sanctum": sanctumCheckboxes
};
const selectedSet = await tp.system.suggester(Object.keys(checkboxSets), Object.keys(checkboxSets));
const checkboxType = checkboxSets[selectedSet];

// Step 2: Let the user choose which checkbox type to use from the selected set  
const check = await tp.system.suggester(checkboxType[0], checkboxType[1], true);

// Step 3: Get the selected text in the file  
const selection = tp.file.selection(); 

// Step 4: If text is selected, replace checkboxes according to user input  
if (selection) {  
    const replace = selection.split("\n").map(select => {  
        // Improved Regex to match - [ ] and similar formats
        if (select.match(/- \[[^\]]?\]/)) {  
            select = select.replace(/- \[[^\]]?\]/, `- [${check}]`);  
        }  
        return select;  
    }).join("\n");  
    
    tR += replace;  
} else {  
    new Notice("Please select text with checkboxes to replace.");  
};
%>
