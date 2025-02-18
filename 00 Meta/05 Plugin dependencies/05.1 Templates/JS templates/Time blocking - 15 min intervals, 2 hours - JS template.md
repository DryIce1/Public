<%*
let start = moment().add(15-(moment().minute()%15),'minutes').startOf('minute');
tR += "- " + start.format('HH:mm') + " - \n";
// moment.js dates are mutable, so always only add 15 mins!
tR += "- " + start.add(15, 'minutes').format('HH:mm') + " - \n";
tR += "- " + start.add(15, 'minutes').format('HH:mm') + " - \n";
tR += "- " + start.add(15, 'minutes').format('HH:mm') + " - \n";
tR += "- " + start.add(15, 'minutes').format('HH:mm') + " - \n";
tR += "- " + start.add(15, 'minutes').format('HH:mm') + " - \n";
tR += "- " + start.add(15, 'minutes').format('HH:mm') + " - \n";
tR += "- " + start.add(15, 'minutes').format('HH:mm') + " - \n";
tR += "- " + start.add(15, 'minutes').format('HH:mm') + " - \n";
%>