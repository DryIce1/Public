<%*
const currentWeeks = new Array(7).fill(null).map((x, i) => {
    // Start from the current week and add each day of the week
    const day = moment(tp.file.title, 'gggg-[W]ww').add(i, 'days');

    // Format each day's log entry as a nested callout
    return `>> [!log]- ## ${day.format('dddd, MMMM Do YYYY')}\n>> ![[${day.format('YYYY-MM-DD')}#Log]]`;
}).join('\n>\n');

// Wrap the weekly logs inside a primary callout
const finalOutput = `> [!date]- daily note logs\n>\n` + currentWeeks + `\n>\n`;

tR += finalOutput;
%>