# Ankit-Sharma_Frontend

                                                       (Answer 1)
                                                     
A simple List component is a commonly used user interface (UI) element that allows for the display and manipulation of a collection of items in a linear, vertical, and horizontal layout. It typically presents a scrollable view of items.

The main purpose of the simple List component is to provide an organized and easily navigable way to present a collection of data to users. 
It often includes features such as sorting, filtering, and selection allowing users to interact with data and perform actions on the items, such as editing, deleting, or rearranging them.

                                                        (Answer 2)

The code provided is a simple List component implemented in React, with some issues and warnings that can be addressed for optimization and correctness. Here are the problems and suggestions for improvement:

PropTypes: The PropTypes used in the code have some issues. PropTypes.array should be PropTypes.arrayOf, and PropTypes.shapeOf should be PropTypes.shape. Additionally, the shape definition is missing a key for text property.

Error:

WrappedListComponent.propTypes = {
 items: PropTypes.array(PropTypes.shapeOf({
   text: PropTypes.string.isRequired,
 })),
};

      Here's the corrected prop type definition for items:

WrappedListComponent.propTypes = {
   items: PropTypes.arrayOf(PropTypes.shape({
     text: PropTypes.string.isRequired,
   })),
 };
 

useState: The useState hook is used incorrectly in the WrappedListComponent. The setSelectedIndex function is used as the first argument in the destructuring assignment, which would cause issues with updating the state correctly.

Error:

 const [setSelectedIndex, selectedIndex] = useState();

The correct usage should be as follows:

const [selectedIndex, setSelectedIndex] = useState(null);



onClickHandler: The onClickHandler function in the WrappedSingleListItem component is not being used correctly. It should be passed as a callback function to the onClick event.

Error:

onClick={onClickHandler(index)}

The correct way to use the function:

onClick={() => onClickHandler(index)}



Key prop: When mapping through an array to generate a list of elements, React requires a unique key prop to be assigned to each item. In the WrappedListComponent, you should add a key prop to the SingleListItem component.

Error:


<SingleListItem
         onClickHandler={() => handleClick(index)}
         text={item.text}
         index={index}
         isSelected={selectedIndex}
       />

Optimized method:

<SingleListItem
 key={index}
 onClickHandler={() => handleClick(index)}
 text={item.text}
 index={index}
 isSelected={selectedIndex}
/>


                                                               (Answer 3)

Here is the modified and optimized code:
The modified code uses correct PropTypes, useState, and onClickHandler usage, and adds a key prop to the mapped components to ensure correct rendering.

import React, { useState, memo, useEffect } from "react";
import PropTypes from "prop-types";
const WrappedSingleListItem = ({ index, isSelected, onClickHandler, text }) => {
 const [bool, setBool] = useState(false);
 useEffect(() => {
   index === isSelected ? setBool(true) : setBool(false);
 }, [index, isSelected]);
 return bool ? (
   <li
     style={{ backgroundColor: "green" }}
     onClick={() => onClickHandler(index)}
   >
     {text}{" "}
   </li>
 ) : (
   <li
     style={{ backgroundColor: "red" }}
     onClick={() => onClickHandler(index)}
   >
     {text}{" "}
   </li>
 );
};
WrappedSingleListItem.propTypes = {
 index: PropTypes.number,
 isSelected: PropTypes.number,
 onClickHandler: PropTypes.func.isRequired,
 text: PropTypes.string.isRequired,
};
const SingleListItem = memo(WrappedSingleListItem);
const WrappedListComponent = ({ items }) => {
 const [selectIndex, setSelectIndex] = useState(false);
 const handleClick = (index) => {
   setSelectIndex(index);
 };
 return (
   <ul style={{ textAlign: "left", cursor: "pointer" }}>
     {items.map((item, index) => (
       <SingleListItem
         onClickHandler={(index) => handleClick(index)}
         text={item.text}
         index={index}
         isSelected={selectIndex}
         key={index}
       />
     ))}
   </ul>
 );
};
WrappedListComponent.propTypes = {
 items: PropTypes.arrayOf(
   PropTypes.shape({
     text: PropTypes.string.isRequired,
   })
 ),
};
WrappedListComponent.defaultProps = {
 items: [
   { text: "HTML" },
   { text: "CSS" },
   { text: "JavaScript" },
   { text: "React" },
 ],
};
const List = memo(WrappedListComponent);
export default List;
